---
layout: post
title: "Navigating the Ocean of LLM (Pre-)Training Data"
---

I write about my journey with infini-gram, OLMoTrace, infini-gram mini, along with several ongoing threads and wishful ideas to pursue.

## N-gram search in massive text corpora

Modern LLMs are pretrained on massive text corpora with many trillions of tokens.
While we don't have access to the training data of the most frontier ones, several openly-released datasets (e.g., Dolma, DCLM, FineWeb) give us some proxy.
Problem is, if you dump a dozen-terabyte dataset on HuggingFace, even if it's "open" to everyone, it's still hard to know *what is* in the dataset.
(The HF dataset search function doesn't work for such huge datasets, as you may have expected.)
The ability to **search** is crucial but absent.

This was the problem Alisa and I ran into when we were working on memorization traps for LLMs.
The original idea was, larger LLMs are more persistent at completing well-known phrases with the common ending word (e.g., completing the proverb "<u>What everybody says must be</u>" with "<u>true</u>"), even when instructed otherwise (e.g., "<u>Write a sentence about challenging common beliefs</u>").
I was curious if the phrase's frequency in the LLM's training data is correlated with such persistence.
But this needs me to count n-grams in a huge corpus, and there wasn't a handy tool for this.

With my competitive programming background, I immediately recognized that this can be efficiently solved with a **suffix array (SA)**.
The main difference is scale -- in CP we build SAs for up to $10^6$ elements, but now we need to deal with $10^{12}$ elements.
This means we need to parallelize the SA index building, and we can't keep all things in RAM simultaneously.

![](/assets/2025-08-02-suffix-array/sa.png)
*Illustration of the suffix array, on a toy example with about 20 elements.*

At the time there were parallelized implementations of SA floating around, the most prominent being the [Rust library written by Lee et al](https://github.com/google-research/deduplicate-text-datasets) for text deduplication.
Many work, including mine, adapt their code for SA indexing (albeit a few bugs and inefficiencies which I fixed while learning to read Rust), and I really appreciate their awesome release.

With the SA built for corpora like the Pile, we had some findings on memorization, but not interesting enough to put together a paper.
This thing sat on my servers for a few months, then one day I chatted with Sewon about this and we decided, "let's use this to build the biggest n-gram LM ever and see what happens!"
Not only did we achieve the biggest in terms of reference data (5 trillion tokens, beating [previous record](https://aclanthology.org/D07-1090.pdf) set by Jeff Dean's team at Google), but we also support arbitrarily large context length $n$ -- hence its name [**infini-gram**](https://infini-gram.io/).
This turned out to be the story we wrote in [our COLM paper](https://arxiv.org/abs/2401.17377), which got mentioned in the 2024 edition of Jurafsky & Martin's [NLP textbook](https://web.stanford.edu/~jurafsky/slp3/) as a modernization of n-gram models.

![](/assets/2025-08-02-suffix-array/slp3.png)
*Infini-gram mentioned in the Speech and Language Processing textbook.*

**I visioned this beyond being a paper.**
This could be a research infrastructure, a tool accessible to everyone to search and learn about LLM training datasets.
Lots of people could use some instant insights to these datasets from time to time, without the overhead of building SA and setting things up.
So in addition to the regular code release, I also built a [web interface](https://infini-gram.io/demo) and a free [API endpoint](https://infini-gram.io/api_doc).
As of July 2025, the API has served over 700 million queries.

Publicly releasing a web interface came with many extra work.
I wrote the original inference engine so that the SA can be used as an n-gram LM, so it had optimized functionalities like counting, computing the next-token distribution, and figuring out the maximum context length $n$.
But these numbers are dull for users to look at.
(Imagine Google Search only tells you how many hits there are, but not all the links and excerpts.)
It'd be much cooler to show the context where the query term appears, and where the document was originally crawled from.

So I went back to tweak the data structure and aligned documents with their metadata.
Folks around me also shared feedback that being able to search for co-occurrence of multiple terms would be super useful, so I invented a fast algorithm to search for [CNF queries](https://en.wikipedia.org/wiki/Conjunctive_normal_form) (which I even forgot to describe in the paper 😅).
<!-- TODO: write a separate blog on CNF. -->

![](/assets/2025-08-02-suffix-array/cnf.png)
*A screenshot from the infini-gram web interface, showing document search with CNF queries.*

Another thing is efficiency.
Users won't like to wait, so it's crucial to decrease latency and increase throughput.
Initially I wrote the inference engine in Python, but later moved to C++ to get true parallelism without having to deal with the GIL.
The first version of my C++ engine communicated with the Python web server via an IPC pipe, which was quite unstable and crashed almost every day.
With the help of Zihao Ye, I moved to [Pybind11](https://github.com/pybind/pybind11) to interface C++ and Python and this has been really good.
<!-- The API has been running stably for a few months without me intervening a single time. -->

C++ also grants me finer-grained low-level I/O control to turbocharge the latency.
One trick is **pre-fetching**.
Since the SA index is too big to fit in RAM, they need to be mmap'ed from disk.
`find()` -- the basic operation underlying all queries in infini-gram -- involves a binary search on the SA, which means about $2 \log N \approx 80$ sequential, random disk reads.
(Well actually it's 2 binary searches, but most disk reads are shared.)
However, the disk reads are not really random and there are patterns to exploit: when binary-searching over an array, at any point we can know the entries we will be looking at in the next, say, $s=3$ steps (there are $2^{s+1} - 2 = 14$ such entries).
If we pre-fetch the values of these entries, the one we really want to look at will likely be ready in RAM when we need it.

With SAs, in each step of the binary search, we don't just compare the value in the SA entry; we need to interpret that entry as an offset in the text dataset and do string comparison with the suffix starting at that offset.
Consequently, we need to pre-fetch the suffix as well, and doing that requires us to already have the SA entry in RAM.
To solve this, I devised a two-tier pre-fetching strategy: at each binary search step, prefetch SA entries $s$ steps ahead, and prefetch the suffix $r$ steps ahead (with $r < s$).
After some tuning on the production server, I found $s = 3$ and $r = 1$ to give the lowest latency.
With the SA index stored on AWS EBS gp3 SSDs (16000 IOPS, 1000 MB/s), the average latency of the `find()` operation is about 20 milliseconds.

![](/assets/2025-08-02-suffix-array/prefetch.png)
*Code for pre-fetching in `find()` operations.*

Setting up the API endpoint caused even more hurdles.
I used the AWS API Gateway to handle rate limits and malicious traffic, but it wasn't easy to correctly chain up all the components like instances, target groups, network load balancers, security groups, API resources, VPCs, and custom domain names.
The API is for batch processing, so it needs to prioritize throughput over latency.
Pre-fetching reduces latency at the cost of burning more disk I/O operations, and at high traffic the disk IOPS (I/O ops per second) becomes the bottleneck (I'll cover this in detail in the OLMoTrace project below), which means I had to turn off pre-fetching for API queries.

As my API starts getting more traffic, more problems surfaced.
One thing I noticed was that my instance would OOM and go down every few days, and the devil turned out to be the **page table**.
Mmap'ing the index from disk doesn't come for free: for every 4K block on disk, a page table entry (8-byte integer) has to live in RAM.
By default, mmap uses a lazy strategy to populate this page table on demand, but as more disk blocks get accessed, the page table grows and there's no apparent way to evict it from RAM.
Eventually, I had to allocate an instance with bigger RAM so that the entire page table can fit.

I want to say there are lots of grunt work that I didn't write about, such as designing a easily-usable API interface, managing versioning between different components, etc.
Overall, I'm really glad that I flashed out all those systems and learning a lot in this journey, and I want to express my deep gratitute to my advisors for kindly offering the cloud credits to let me keep the service running.

<!-- // Bugs: (1) merge did not skip empty shard; (2) checks length of 5M instead of HACKSIZE; (3) concat twice; (4) docid can exceed intmax -->

<!-- add metadata -->

<!-- inference engine: python=>c++, pybind11, ntd, infinity-gram LM, pre-fetch, co-occurrence -->

<!-- API, set up website, rate limit, concurrency, stability (page table) -->

<!-- Design API interface, versioning, ... -->

## Connecting LLM outputs to their training data

Infini-gram was a splash, but to use it you need to clearly know what to search for.
At the same time, why LLMs generate the outputs they do was still largely a mystery, and it was intertwined with discussions on copyright and AI creativity.
Can infini-gram contribute to this challenge by directly connecting LLM outputs to their training data?

If we can find long pieces of LLM outputs that have appeared *verbatim* in its training data, in many cases it is pretty good insight that the LLM may have learned such token sequences from these training documents alike.
This can be a "data tracing" tool that complements things like influence functions (which is not scalable) and mech interp.
I was messing around with this idea in mid 2024, and eventually I joined efforts with Ai2 to build this tool, [**OLMoTrace**](https://allenai.org/blog/olmotrace).

![](/assets/2025-08-02-suffix-array/olmotrace.png)
*A screenshot of OLMoTrace. Highlighted spans in the model response appear verbatim in the model's training data.*

### The techincal part

Apparently, we can't expect a multi-hundred-token LLM output to exist contiguously in the training data (unless it's regurgitating some well-known stuff), so we should look for substrings (i.e., spans of tokens) of the LLM output that do exist.
Since the training data is so huge, we can actually find a lot of long spans (e.g., 10 tokens or more) with a match.
But say the LLM output has $L$ tokens, do we enumerate all $O(L^2)$ spans and query infini-gram?

Well, we *could* parallelize these queries, but we'll hit the IOPS limit of disks.
A back-of-the-envelope calculation: Each `find()` is 80 disk reads, and we need to multiply by 12 because the data is so huge that we need to shard the SA 12 ways; each LLM output is about 450 tokens; this gives us $80 \times 12 \times (450^2 / 2) = 97M$ disk IOs.
The standard SSD on GCP has 80k IOPS, so processing each LLM output would take 20 minutes.
This is unacceptable.

There's an obvious monotonicity: If we already know a shorter span doesn't exist, we don't need to check the longer spans enclosing it.
We adopted this heuristic when using infini-gram to compute the [Creativity Index](https://arxiv.org/abs/2410.04265) of text, which we define based on n-gram novelty.
The algorithm, which we dubbed as "DJ Search", reduces the queries from $L^2 / 2$ to $2L$ sequential ones, and considering each query being 20ms this gives us $(2 \times 450) \times 20 \text{ms} = 18$ seconds per LLM output.
It was good enough for running research experiments offline.

![](/assets/2025-08-02-suffix-array/dj.png)
*A sketch of the DJ Search algo. Each marked cell represents a query to infini-gram.*

But it wasn't good enough for real-time serving.
I want this to be part of a LLM chat inferface, and the match results should pop up right after the LLM finishes generation.
The problem with DJ Search is that the queries need to be made sequentially, stacking up the latency.
So for OLMoTrace I came up with a new smart algo that reduced to $L$ queries that can be parallelized.
The key idea is that we only need to find the **"maximal matching spans"** in the LLM output that exist in the training data, which can be reduced to making one `find()` query per suffix of the LLM output.
Interested readers can dig into [our paper](https://arxiv.org/abs/2504.07096) for details.
The final latency landed at 4.5 seconds per LLM output.

![](/assets/2025-08-02-suffix-array/span.png)
*A sketch of the fast algorithm for finding "maximal matching spans" in OLMoTrace.*

Side note: As we scale up the parallelism, we hit another limit -- the number of threads that can be created in the Linux system.
By default, most machines give us 1 million threads per process, which is pretty generous, but when running OLMoTrace we actually need to watch out for this.
It seems that whenever we scale things up a magnitude, some unexpected bottleneck may emerge 🙃

<!-- connecting, copyright, creativity index -->

<!-- thread limits -->

### The product part

Actually, by mid 2024 I've already flashed out the core technical part of OLMoTrace.
But we didn't release until April 2025, and this was mainly because we've been polishing it as a product.
We want to make OLMoTrace a user-friendly tool that enhances LLM transparency, and that came with lots of considerations.

First thing was to reduce confusion for users.
N-gram matching doesn't account for semantics, and thus sometimes the context where the matched spans appear in the training data can be irrelevant to the LLM output.
For example, if the LLM says "<u>Celine Dion has been involved in philanthropy</u>", we may find "<u>has been involved in philanthropy</u>" in the training data but for describing another person.
If a user sees this training document on the top, they may get the wrong message from our tool.
To address this, we applied a reranker to surface the most relevant matched training documents in the UI.
We found a BM25 reranker to be roughly as good as neural embedding models in terms of perceived relevance (via human evaluation), so we went with BM25 to avoid needing GPU machines in the production system.

We also decluttered the UI so as to not overwhelm users.
There can be many "maximal matching spans" to show, some of which overlapping, which would be both challenging and confusing to highlight.
We filtered the spans to only keep relatively long and unique ones (which are more likely to be worth inspecting), and did some merging of overlapping spans.
We enforced spans to not start/end in the middle of a word or cross sentence/paragraph boundaries, because they look weird.
We deduplicated the matched training documents, which would have spammed the UI.

We went through several rounds of **bug bashes** with the AllenNLP team.
To integrate into a chat interface, there are numerous things we need to consider.
What if there are multiple turns in the chat?
What if there are contents rendered as code block / latex / markdown?
What if there are Unicode characters?
These are just a sample of things we had to nail down before release.
We also ran an internal **red teaming** to understand and mitigate legal risks, including copyrighted books, lyrics, and toxic content.

**Gradually I came to realize, to ship a great product, we've come a long way grinding numerous small aspects so that it finally meets the bar.**
It is very different from research.

<!-- core functionality, latency, story / usability / relevance, UI interactions, human eval -->

<!-- overlapping spans, word boundary, merge and dedup docs, unigram filtering -->

<!-- bug bash, legal & red teaming -->

<!-- To ship this product, we’re grinding multiple small aspects so that it finally will meet the bar … So different from research -->

### The teamwork part

OLMoTrace is a huge team effort.
For me, it's been a unique experience to be the "tech lead" of a big team, cross-functioning across and bring together partners from engineering, research, design, comms, legal, and company leadership.

Working as a team means I need to forego my bad research-y coding habits, and instead write unit tests, lint my code, create and review PRs, etc.
We also use project trackers and meet weekly to prioritize tickets.

During the few months, some people resigns and some new people join -- we had a complete rotation of designers, and our PM was also assigned back-and-forth -- and I had to navigate that.
I also grew to be more aware of people's individual career goals and what they wish to get from the project: someone may want to become a lead engineer; someone may want to build up as a senior research advisor; the design team may want to make an impact by unifying stuff under the new company brand.
I spent some effort to align these with their role on the project and thus motivate the team.

One big part was to keep sync'ing with stakeholders and reconciling the many, sometimes conflicting, feedback.
Most of the feedback were great and we incorporated them.
I also had my vision of the project, and for those feedback that doesn't go well with that vision, I would try to convince people.
Of course, I also get convinced or compromise from time to time.

An interesting lesson I learned was how to communicate with leadership.
Basically I need to be more prepared and less unhinged than chatting with my PhD advisors.
Leadership is like your first user: they're busy, they make a judgment based on first impression, and it's easy for them to get the wrong message.
Sometimes it means the opening sentence sets the stage of whether they'll get it or not.
Cutting straight to a demo may preempt the convo from going to a direction I don't like.
Sometimes it means having to asking them to look at somewhat cherry-picked examples before we iron out use cases more broadly.

<!-- unit tests, lint, PR, multiple repos, project trackers, prioritize, talking to stakeholders, people come and go, people's career goal (eng lead, tech lead, advising, design)

reconciling conflicting feedback
* click anywhere to cancel selection
* select custom spans
* tunable span density -->

<!-- how to communicate with leadership, can't be unhinged -->

## Compress more, index more

Concurrently to OLMoTrace, I've been thinking how we can make even larger text corpora searchable, and in particular, Common Crawl.
As the source corpus for most pretraining datasets (if not all), Common Crawl contains about 1 PB of text and continues to grow every month.
If we index this corpus, we'd be able to understand (a large part of) the pre-training data of most LLMs, including proprietary ones.
However, storing the SA index (on cloud) alone would cost $560k per month.
Well, we're not Google, and we need to stay with a reasonable budget.

I had a call with Christina Boucher from U Florida last year and she introduced me to a data structure called [FM-index](https://en.wikipedia.org/wiki/FM-index).
It is a compressed version of the SA index: instead of storing the full text corpus and its suffix array, FM-index store a subsampled version of the suffix array and a compressed version of some permutation of the text corpus (called the [BWT](https://en.wikipedia.org/wiki/Burrows%E2%80%93Wheeler_transform)).
This gives tremendous storage save -- up to 27x compared to SA.

![](/assets/2025-08-02-suffix-array/mini.png)
*Infini-gram mini uses FM-index, a more storage-efficient data structure that is similarly powerful as SA.*

The best existing implementation of FM-index was [SDSL](https://github.com/simongog/sdsl-lite) from a decade ago.
It has only been tested on datasets beyond a hundred GB large, doesn't have multi-CPU parallel indexing, and no on-disk inference.
Working with Hao Xu, an undergrad student at UW, we did extensive engineering to overcome these bottlenecks.
Our system, [**infini-gram mini**](https://infini-gram-mini.io/), speeds up indexing by 18x and reduces RAM use by 3.2x compared to SDSL.
**With the lower storage multiplier, we can now index bigger corpora.**
In total, we indexed 46TB of text, including the first 3 months of Common Crawl from 2025.
(Similar to infini-gram, there's [web interface](https://infini-gram-mini.io/demo) and [API](https://infini-gram-mini.io/docs) for querying these corpora.)
Indexing the entire Common Crawl is also within reach -- if you'd like to sponsor this effort, please shoot me an email and I'd love to burn some of your cloud credits 💸

We used infini-gram mini to analyze and monitor the contamination of LLM benchmarks in Common Crawl.
We found heavy contamination of widely-used benchmarks like MMLU and SQuAD.
Math and coding benchmarks are relatively clean, but current practices in benchmark publishing and data crawling almost guarantee that they will get increasingly contaminated.
We created a public [bulletin](https://infini-gram-mini.io/bulletin) to monitor this situation over time as more crawls are made available.

![](/assets/2025-08-02-suffix-array/bulletin.png)
*The bulletin is updated monthly with new crawls from Common Crawl, and anyone can submit new benchmarks to be monitored.*

<!-- indexing CC, too expensive some math
Christina Boucher, FM-index
Intro to FM-index
our optimizations
use cases, contam -->

## Open-source scalable deduplication

## Combining neural LLMs with n-gram models?
