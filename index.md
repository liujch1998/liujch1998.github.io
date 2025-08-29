---
layout: home
---

<img src='/assets/Profile.png' width=250 style="float: right;">

I am a `(Now() - 04/2021).ceil().ordinal()` year PhD student studying AI and language models at University of Washington.
I am fortunate to be advised by Prof. Yejin Choi and Prof. Hanna Hajishirzi.
I am also a student researcher at the Allen Institute for AI (Ai2).

My current research topics are inspecting massive text corpora, training data attribution, LM pretraining, and scaling laws.
During my PhD, I have worked on commonsense knowledge generation and verification, automated theorem proving, RLHF, and text decoding.

Previously, I received B.S. in Computer Science from University of Illinois at Urbana-Champaign, where I worked with Prof. Julia Hockenmaier.
I used to work in Facebook's Natural Language Generation (NLG) team.

My name in Chinese characters is 刘嘉程

Email: liujc [at] cs [dot] washington [dot] edu

[[CV](/assets/cv/cv.pdf)]
[[Google Scholar](https://scholar.google.com/citations?user=GJfoBZAAAAAJ)]
[[GitHub](https://github.com/liujch1998)]
[[Twitter](https://twitter.com/liujc1998)]
[[LinkedIn](https://www.linkedin.com/in/liujch1998/)]

<!--
Research and other blogs: this website and
[[Zhihu](https://www.zhihu.com/people/liujch1998)]

Private pilot and other personal life VLOGs:
[[Bilibili](https://space.bilibili.com/326361080)]
[[YouTube](https://www.youtube.com/channel/UCG06GwB1IK0bXXrTcRe5Elw)]

Personal:
[[Facebook](https://www.facebook.com/liujch1998/)]
-->

---

# News

* (2025.07) **OLMoTrace** received the Best Demo Award at ACL 2025! 🏆
* (2025.07) Talk at TPC25 (Trillion Parameter Consortium)
* (2025.07) Talk at Stanford NLP
* (2025.06) Talk at IBM Research
* (2025.05) Talk at UCSD
* (2025.04) Introducing [**OLMoTrace**](https://arxiv.org/pdf/2504.07096): tracing LLM outputs back into their multi-trillion-token training data in real time. Now available on [Ai2 Playground](https://playground.allenai.org)
* (2024.07) [**Infini-gram**](https://arxiv.org/pdf/2401.17377.pdf) and [**PPO-MCTS**](https://arxiv.org/pdf/2309.15028.pdf) are accepted to COLM 2024.
* (2024.01) Introducing [**infini-gram**](https://arxiv.org/pdf/2401.17377.pdf): an efficient text search engine, and the biggest n-gram LM ever built.
* (2023.10) [**PPO-MCTS**](https://arxiv.org/pdf/2309.15028.pdf) is [featured by](https://mp.weixin.qq.com/s/Zat0cOa5C8S8O5qEEbLcrg) 机器之心 on WeChat!
* (2023.10) [**Vera**](https://arxiv.org/pdf/2305.03695.pdf) and [**Crystal**](https://arxiv.org/pdf/2310.04921.pdf) are accepted to EMNLP 2023 (main conference).
* (2023.09) The [Inverse Scaling](https://arxiv.org/pdf/2306.09479.pdf) paper is accepted to TMLR! Check out our contributed dataset, `memo-trap`, where LLMs demonstrate the strongest inverse scaling trends.
* (2023.07) I am awarded the [Qualcomm Innovation Fellowship](https://www.qualcomm.com/research/university-relations/innovation-fellowship/2023-north-america) for academic year 2023-2024.
* (2023.05) Invited talk the the MLNLP Seminar: [*Estimating the plausibility of commonsense statements*](https://mp.weixin.qq.com/s/yEqKWY8ksX_3NwZ15QXQYw).
* (2023.02) Our submission to the [Inverse Scaling Challenge](https://github.com/inverse-scaling/prize), `memo-trap`, [receives](https://irmckenzie.co.uk/round2) one of the 11 Third Prizes!

---

# Selected Publications

See my full list of publications [here](/publications.md).

[**OLMoTrace: Tracing Language Model Outputs Back to Trillions of Training Tokens**](https://arxiv.org/pdf/2504.07096) \\
**Jiacheng Liu**, Taylor Blanton, Yanai Elazar, Sewon Min, YenSung Chen, Arnavi Chheda-Kothary, Huy Tran, Byron Bischoff, Eric Marsh, Michael Schmitz, Cassidy Trier, Aaron Sarnat, Jenna James, Jon Borchardt, Bailey Kuehl, Evie Cheng, Karen Farley, Sruthi Sreeram, Taira Anderson, David Albright, Carissa Schoenick, Luca Soldaini, Dirk Groeneveld, Rock Yuren Pang, Pang Wei Koh, Noah A. Smith, Sophie Lebrecht, Yejin Choi, Hannaneh Hajishirzi, Ali Farhadi, Jesse Dodge \\
ACL 2025 System Demonstrations Track **(Best Demo Award)** \\
[[Arxiv](https://arxiv.org/pdf/2504.07096)]
[[Blog](https://allenai.org/blog/olmotrace)]
[[Web Interface](https://playground.allenai.org)]
[[Code](https://github.com/allenai/infinigram-api)]
[[Twitter](https://x.com/liujc1998/status/1909963236864299068)]
[[Trailer Video](https://www.youtube.com/watch?v=A71RSAxjVqc)]
[[Demo Video](https://www.youtube.com/watch?v=wyLRWza_v9M)]

[**Infini-gram mini: Exact n-gram Search at the Internet Scale with FM-Index**](https://arxiv.org/pdf/2506.12229) \\
Hao Xu, **Jiacheng Liu**, Yejin Choi, Noah A. Smith, Hannaneh Hajishirzi \\
EMNLP 2025 (Main Conference) \\
[[Arxiv](https://arxiv.org/pdf/2506.12229)]
[[Project Page](https://infini-gram-mini.io/)]
[[Web Interface](https://huggingface.co/spaces/infini-gram-mini/infini-gram-mini)]
[[API Endpoint](https://api.infini-gram-mini.io/)]
[[Code](https://github.com/xuhaoxh/infini-gram-mini)]
[[Documentation](https://infini-gram-mini.readthedocs.io)]
[[Contamination Bulletin](https://huggingface.co/spaces/infini-gram-mini/Benchmark-Contamination-Monitoring-System)]

[**Infini-gram: Scaling Unbounded n-gram Language Models to a Trillion Tokens**](https://openreview.net/pdf?id=u2vAyMeLMm) \\
**Jiacheng Liu**, Sewon Min, Luke Zettlemoyer, Yejin Choi, Hannaneh Hajishirzi \\
COLM 2024 (Oral Spotlight, 2%) \\
[[Arxiv](https://arxiv.org/pdf/2401.17377.pdf)]
[[Project Page](https://infini-gram.io/)]
[[Web Interface](https://huggingface.co/spaces/liujch1998/infini-gram)]
[[API Endpoint](https://api.infini-gram.io/)]
[[Python Package](https://pypi.org/project/infini-gram)]
[[Code](https://github.com/liujch1998/infini-gram)]
[[Documentation](https://infini-gram.readthedocs.io)]

[**Unpacking DPO and PPO: Disentangling Best Practices for Learning from Preference Feedback**](https://arxiv.org/pdf/2406.09279) \\
Hamish Ivison, Yizhong Wang, **Jiacheng Liu**, Zeqiu Wu, Valentina Pyatkin, Nathan Lambert, Noah A Smith, Yejin Choi, Hannaneh Hajishirzi \\
NeurIPS 2024 \\
[[Arxiv](https://arxiv.org/pdf/2406.09279)]
[[Code](https://github.com/hamishivi/EasyLM)]
[[Models](https://huggingface.co/collections/allenai/tulu-v25-suite-66676520fd578080e126f618)]

[**Don't throw away your value model! Generating more preferable text with Value-Guided Monte-Carlo Tree Search decoding**](https://openreview.net/pdf?id=kh9Zt2Ldmn) \\
**Jiacheng Liu**, Andrew Cohen, Ramakanth Pasunuru, Yejin Choi, Hannaneh Hajishirzi, Asli Celikyilmaz \\
COLM 2024 \\
[[Arxiv](https://arxiv.org/pdf/2309.15028.pdf)]
[[Code](https://github.com/liujch1998/ppo-mcts)]

[**Vera: A General-Purpose Plausibility Estimation Model for Commonsense Statements**](https://aclanthology.org/2023.emnlp-main.81.pdf) \\
**Jiacheng Liu**, Wenya Wang, Dianzhuo Wang, Noah A. Smith, Yejin Choi, Hannaneh Hajishirzi \\
EMNLP 2023 (Main Conference, Oral) \\
[[Arxiv](https://arxiv.org/pdf/2305.03695.pdf)]
[[Code](https://github.com/liujch1998/vera)]
[[Model](https://huggingface.co/liujch1998/vera)]
[[Demo](https://huggingface.co/spaces/liujch1998/vera)]
[[Dataset](https://huggingface.co/datasets/liujch1998/vera_contrib)]

[**Generated Knowledge Prompting for Commonsense Reasoning**](https://aclanthology.org/2022.acl-long.225.pdf) \\
**Jiacheng Liu**, Alisa Liu, Ximing Lu, Sean Welleck, Peter West, Ronan Le Bras, Yejin Choi, Hannaneh Hajishirzi \\
ACL 2022 (Main Conference) \\
[[Arxiv](https://arxiv.org/pdf/2110.08387.pdf)]
[[Code](https://github.com/liujch1998/GKP)]
[[Talk](https://underline.io/events/284/sessions/10764/lecture/50402-long-generated-knowledge-prompting-for-commonsense-reasoning)]
[[Poster](https://underline.io/events/284/sessions/10764/lecture/50402-long-generated-knowledge-prompting-for-commonsense-reasoning)]
