---
layout: home
---

<img src='/assets/Profile.png' width=250 style="float: right;">

I am a `(Now() - 04/2021).ceil().ordinal()` year PhD student studying natural language processing (NLP) at University of Washington.
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

<br/>

# News

* (2025.04) Introducing [**OLMoTrace**](https://arxiv.org/pdf/2504.07096): tracing LLM outputs back into their multi-trillion-token training data in real time. Now available on [Ai2 Playground](https://playground.allenai.org)
* (2024.07) [**Infini-gram**](https://arxiv.org/pdf/2401.17377.pdf) and [**PPO-MCTS**](https://arxiv.org/pdf/2309.15028.pdf) are accepted to COLM 2024.
* (2024.01) Introducing [**infini-gram**](https://arxiv.org/pdf/2401.17377.pdf): an efficient text search engine, and the biggest n-gram LM ever built.
* (2023.10) [**PPO-MCTS**](https://arxiv.org/pdf/2309.15028.pdf) is [featured by](https://mp.weixin.qq.com/s/Zat0cOa5C8S8O5qEEbLcrg) 机器之心 on WeChat!
* (2023.10) [**Vera**](https://arxiv.org/pdf/2305.03695.pdf) and [**Crystal**](https://arxiv.org/pdf/2310.04921.pdf) are accepted to EMNLP 2023 (main conference).
* (2023.09) The [Inverse Scaling](https://arxiv.org/pdf/2306.09479.pdf) paper is accepted to TMLR! Check out our contributed dataset, `memo-trap`, where LLMs demonstrate the strongest inverse scaling trends.
* (2023.07) I am awarded the [Qualcomm Innovation Fellowship](https://www.qualcomm.com/research/university-relations/innovation-fellowship/2023-north-america) for academic year 2023-2024.
* (2023.05) Invited talk the the MLNLP Seminar: [*Estimating the plausibility of commonsense statements*](https://mp.weixin.qq.com/s/yEqKWY8ksX_3NwZ15QXQYw).
* (2023.02) Our submission to the [Inverse Scaling Challenge](https://github.com/inverse-scaling/prize), `memo-trap`, [receives](https://irmckenzie.co.uk/round2) one of the 11 Third Prizes!

# Publications

## Preprints

[Infini-gram mini: Exact n-gram Search at the Internet Scale with FM-Index](https://arxiv.org/pdf/2506.12229) \\
Hao Xu, **Jiacheng Liu**, Yejin Choi, Noah A. Smith, Hannaneh Hajishirzi \\
[[Arxiv](https://arxiv.org/pdf/2506.12229)]
[[Project Page](https://infini-gram-mini.io/)]
[[Web Interface](https://huggingface.co/spaces/infini-gram-mini/infini-gram-mini)]
[[API Endpoint](https://api.infini-gram-mini.io/)]
[[Code](https://github.com/xuhaoxh/infini-gram-mini)]
[[Documentation](https://infini-gram-mini.readthedocs.io)]
[[Contamination Bulletin](https://huggingface.co/spaces/infini-gram-mini/Benchmark-Contamination-Monitoring-System)]

[2 OLMo 2 Furious](https://arxiv.org/pdf/2501.00656) \\
Team OLMo, Pete Walsh, Luca Soldaini, Dirk Groeneveld, Kyle Lo, ..., **Jiacheng Liu**, ..., Ali Farhadi, Noah A. Smith, Hannaneh Hajishirzi \\
[[Arxiv](https://arxiv.org/pdf/2501.00656)]
[[Models/Datasets](https://huggingface.co/collections/allenai/olmo-2-674117b93ab84e98afc72edc)]
[[Code](https://github.com/allenai/OLMo)]
[[Demo](https://playground.allenai.org)]

[Establishing Task Scaling Laws via Compute-Efficient Model Ladders](https://arxiv.org/pdf/2412.04403) \\
Akshita Bhagia*, **Jiacheng Liu\***, Alexander Wettig, David Heineman, Oyvind Tafjord, Ananya Harsh Jha, Luca Soldaini, Noah A. Smith, Dirk Groeneveld, Pang Wei Koh, Jesse Dodge, Hannaneh Hajishirzi \\
[[Arxiv](https://arxiv.org/pdf/2412.04403)]

## Peer-Reviewed Papers

[OLMoTrace: Tracing Language Model Outputs Back to Trillions of Training Tokens](https://arxiv.org/pdf/2504.07096) \\
**Jiacheng Liu**, Taylor Blanton, Yanai Elazar, Sewon Min, YenSung Chen, Arnavi Chheda-Kothary, Huy Tran, Byron Bischoff, Eric Marsh, Michael Schmitz, Cassidy Trier, Aaron Sarnat, Jenna James, Jon Borchardt, Bailey Kuehl, Evie Cheng, Karen Farley, Sruthi Sreeram, Taira Anderson, David Albright, Carissa Schoenick, Luca Soldaini, Dirk Groeneveld, Rock Yuren Pang, Pang Wei Koh, Noah A. Smith, Sophie Lebrecht, Yejin Choi, Hannaneh Hajishirzi, Ali Farhadi, Jesse Dodge \\
ACL 2025 System Demonstrations Track \\
[[Arxiv](https://arxiv.org/pdf/2504.07096)]
[[Blog](https://allenai.org/blog/olmotrace)]
[[Web Interface](https://playground.allenai.org)]
[[Code](https://github.com/allenai/infinigram-api)]
[[Twitter](https://x.com/liujc1998/status/1909963236864299068)]
[[Trailer Video](https://www.youtube.com/watch?v=A71RSAxjVqc)]
[[Demo Video](https://www.youtube.com/watch?v=wyLRWza_v9M)]

[Few-shot viral variant detection via bayesian active learning and biophysics](https://www.biorxiv.org/content/biorxiv/early/2025/03/13/2025.03.12.642881.full.pdf) \\
Marian Huot, Dianzhuo Wang, **Jiacheng Liu**, Eugene Shakhnovich \\
PNAS \\
[[Arxiv](https://www.biorxiv.org/content/biorxiv/early/2025/03/13/2025.03.12.642881.full.pdf)]

[AI as Humanity's Salieri: Quantifying Linguistic Creativity of Language Models via Systematic Attribution of Machine Text against Web Text](https://arxiv.org/pdf/2410.04265) \\
Ximing Lu, Melanie Sclar, Skyler Hallinan, Niloofar Mireshghallah, **Jiacheng Liu**, Seungju Han, Allyson Ettinger, Liwei Jiang, Khyathi Chandu, Nouha Dziri, Yejin Choi \\
ICLR 2025 (Oral) \\
[[Arxiv](https://arxiv.org/pdf/2410.04265)]
[[Code](https://github.com/GXimingLu/creativity_index)]
[[Demo](https://huggingface.co/spaces/liujch1998/creativity)]

[Unpacking DPO and PPO: Disentangling Best Practices for Learning from Preference Feedback](https://arxiv.org/pdf/2406.09279) \\
Hamish Ivison, Yizhong Wang, **Jiacheng Liu**, Zeqiu Wu, Valentina Pyatkin, Nathan Lambert, Noah A Smith, Yejin Choi, Hannaneh Hajishirzi \\
NeurIPS 2024 \\
[[Arxiv](https://arxiv.org/pdf/2406.09279)]
[[Code](https://github.com/hamishivi/EasyLM)]
[[Models](https://huggingface.co/collections/allenai/tulu-v25-suite-66676520fd578080e126f618)]

[Infini-gram: Scaling Unbounded n-gram Language Models to a Trillion Tokens](https://openreview.net/pdf?id=u2vAyMeLMm) \\
**Jiacheng Liu**, Sewon Min, Luke Zettlemoyer, Yejin Choi, Hannaneh Hajishirzi \\
COLM 2024 (Oral Spotlight, 2%) \\
[[Arxiv](https://arxiv.org/pdf/2401.17377.pdf)]
[[Project Page](https://infini-gram.io/)]
[[Web Interface](https://huggingface.co/spaces/liujch1998/infini-gram)]
[[API Endpoint](https://api.infini-gram.io/)]
[[Python Package](https://pypi.org/project/infini-gram)]
[[Code](https://github.com/liujch1998/infini-gram)]
[[Documentation](https://infini-gram.readthedocs.io)]

[Don't throw away your value model! Making PPO even better via Value-Guided Monte-Carlo Tree Search decoding](https://openreview.net/pdf?id=kh9Zt2Ldmn) \\
**Jiacheng Liu**, Andrew Cohen, Ramakanth Pasunuru, Yejin Choi, Hannaneh Hajishirzi, Asli Celikyilmaz \\
COLM 2024 \\
[[Arxiv](https://arxiv.org/pdf/2309.15028.pdf)]
[[Code](https://github.com/liujch1998/ppo-mcts)]

[Are machines better at complex reasoning? Unveiling human-machine inference gaps in entailment verification](https://aclanthology.org/2024.findings-acl.618.pdf) \\
Soumya Sanyal, Tianyi Xiao, **Jiacheng Liu**, Wenya Wang, Xiang Ren \\
ACL 2024 (Findings) \\
[[Arxiv](https://arxiv.org/pdf/2402.03686)]
[[Model](https://huggingface.co/soumyasanyal/nli-entailment-verifier-xxl)]

[MathVista: Evaluating Mathematical Reasoning of Foundation Models in Visual Contexts](https://openreview.net/pdf?id=KUNzEQMWU7) \\
Pan Lu, Hritik Bansal, Tony Xia, **Jiacheng Liu**, Chunyuan Li, Hannaneh Hajishirzi, Hao Cheng, Kai-Wei Chang, Michel Galley, Jianfeng Gao \\
ICLR 2024 (Oral); NeurIPS 2023 MATH-AI Workshop \\
[[Arxiv](https://arxiv.org/pdf/2310.02255.pdf)]
[[Project Page](https://mathvista.github.io)]
[[Code](https://github.com/lupantech/MathVista)]
[[Dataset](https://drive.google.com/file/d/1jX_nKaoDALEttiN1IR0dr89qLVt8yBkO/view)]
[[HF Dataset](https://huggingface.co/datasets/AI4Math/MathVista)]

[Crystal: Introspective Reasoners Reinforced with Self-Feedback](https://aclanthology.org/2023.emnlp-main.708.pdf) \\
**Jiacheng Liu**, Ramakanth Pasunuru, Hannaneh Hajishirzi, Yejin Choi, Asli Celikyilmaz \\
EMNLP 2023 (Main Conference, Oral) \\
[[Arxiv](https://arxiv.org/pdf/2310.04921.pdf)]
[[Code](https://github.com/liujch1998/crystal)]
[Models: [large](https://huggingface.co/liujch1998/crystal-large) [3b](https://huggingface.co/liujch1998/crystal-3b) [11b](https://huggingface.co/liujch1998/crystal-11b)]
[[Demo](https://huggingface.co/spaces/liujch1998/crystal)]

[Vera: A General-Purpose Plausibility Estimation Model for Commonsense Statements](https://aclanthology.org/2023.emnlp-main.81.pdf) \\
**Jiacheng Liu**, Wenya Wang, Dianzhuo Wang, Noah A. Smith, Yejin Choi, Hannaneh Hajishirzi \\
EMNLP 2023 (Main Conference, Oral) \\
[[Arxiv](https://arxiv.org/pdf/2305.03695.pdf)]
[[Code](https://github.com/liujch1998/vera)]
[[Model](https://huggingface.co/liujch1998/vera)]
[[Demo](https://huggingface.co/spaces/liujch1998/vera)]
[[Dataset](https://huggingface.co/datasets/liujch1998/vera_contrib)]

[Inverse Scaling: When Bigger Isn't Better](https://openreview.net/pdf?id=DwgRm72GQF) \\
Ian R McKenzie, ..., **Jiacheng Liu**, ..., Samuel R Bowman, Ethan Perez \\
TMLR (2023.10) \\
[[Arxiv](https://arxiv.org/pdf/2306.09479.pdf)]

[Draft, Sketch, and Prove: Guiding Formal Theorem Provers with Informal Proofs](https://openreview.net/pdf?id=SMa9EAovKMC) \\
Albert Qiaochu Jiang, Sean Welleck, Jin Peng Zhou, Timothee Lacroix, **Jiacheng Liu**, Wenda Li, Mateja Jamnik, Guillaume Lample, Yuhuai Wu \\
ICLR 2023 (Oral, 5%) \\
[[Arxiv](https://arxiv.org/pdf/2210.12283.pdf)]

[Rainier: Reinforced Knowledge Introspector for Commonsense Question Answering](https://aclanthology.org/2022.emnlp-main.611.pdf) \\
**Jiacheng Liu**, Skyler Hallinan, Ximing Lu, Pengfei He, Sean Welleck, Hannaneh Hajishirzi, Yejin Choi \\
EMNLP 2022 (Main Conference) \\
[[Arxiv](https://arxiv.org/pdf/2210.03078.pdf)]
[[Code/Data](https://github.com/liujch1998/rainier)]
[Models: [Policy](https://huggingface.co/liujch1998/rainier-large) [Value](https://huggingface.co/liujch1998/rainier-large-value)]
[[Demo](https://huggingface.co/spaces/liujch1998/rainier)]
<!-- [[Demo](http://qa.cs.washington.edu:14411/)] -->

[NaturalProver: Grounded Mathematical Proof Generation with Language Models](https://papers.nips.cc/paper_files/paper/2022/file/1fc548a8243ad06616eee731e0572927-Paper-Conference.pdf) \\
Sean Welleck, **Jiacheng Liu**, Ximing Lu, Hannaneh Hajishirzi, Yejin Choi \\
NeurIPS 2022 \\
[[Arxiv](https://arxiv.org/pdf/2205.12910.pdf)]
[[Code](https://github.com/wellecks/naturalprover)]

[NaturalProver: Grounded Natural Language Proof Generation with Language Models](http://aitp-conference.org/2022/abstract/AITP_2022_paper_12.pdf) \\
Sean Welleck, **Jiacheng Liu**, Ximing Lu, Hannaneh Hajishirzi, Yejin Choi \\
AITP 2022 (Contributed Talk) \\
[[Talk](http://grid01.ciirc.cvut.cz/~mptp/zoomaitp/2022-09-07/video1696925438.mp4)]

[Generated Knowledge Prompting for Commonsense Reasoning](https://aclanthology.org/2022.acl-long.225.pdf) \\
**Jiacheng Liu**, Alisa Liu, Ximing Lu, Sean Welleck, Peter West, Ronan Le Bras, Yejin Choi, Hannaneh Hajishirzi \\
ACL 2022 (Main Conference) \\
[[Arxiv](https://arxiv.org/pdf/2110.08387.pdf)]
[[Code](https://github.com/liujch1998/GKP)]
[[Talk](https://underline.io/events/284/sessions/10764/lecture/50402-long-generated-knowledge-prompting-for-commonsense-reasoning)]
[[Poster](https://underline.io/events/284/sessions/10764/lecture/50402-long-generated-knowledge-prompting-for-commonsense-reasoning)]

[Towards Grounded Natural Language Proof Generation](https://mathai4ed.github.io/papers/papers/paper_10.pdf) \\
Sean Welleck, **Jiacheng Liu**, Jesse Michael Han, Yejin Choi \\
NeurIPS 2021 MATHAI4ED Workshop (Contributed Talk) \\
[[Talk](https://neurips.cc/virtual/2021/workshop/21828)]
[[Poster](https://mathai4ed.github.io/papers/posters/poster_10.png)]

[NaturalProofs: Mathematical Theorem Proving in Natural Language](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/file/d9d4f495e875a2e075a1a4a6e1b9770f-Paper-round1.pdf) \\
Sean Welleck, **Jiacheng Liu**, Ronan Le Bras, Hannaneh Hajishirzi, Yejin Choi, Kyunghyun Cho \\
NeurIPS 2021 Datasets and Benchmarks Track (Oral, 1%) \\
[[Arxiv](https://arxiv.org/pdf/2104.01112.pdf)]
[[Data/Code/Models](https://github.com/wellecks/naturalproofs)]
[[Project Page](https://wellecks.github.io/naturalproofs/)]
[[Talk](https://nips.cc/Conferences/2021/ScheduleMultitrack?event=38450)]
[[Slides](https://drive.google.com/file/d/1ZqN_ClsP6Y1_aFVsW_61pcvgcpJ3E4m9/view)]

[NaturalProofs: Mathematics meets Natural Language](http://aitp-conference.org/2021/abstract/paper_4.pdf) \\
Sean Welleck, **Jiacheng Liu**, Ronan Le Bras, Hannaneh Hajishirzi, Yejin Choi, Kyunghyun Cho \\
AITP 2021 (Contributed Talk) \\
[[Talk](http://grid01.ciirc.cvut.cz/~mptp/zoomaitp/2021-09-07a/zoom_1.mp4)]
[[Slides](http://aitp-conference.org/2021/slides/SW.pdf)]

[Phrase Grounding by Soft-Label Chain Conditional Random Field](https://www.aclweb.org/anthology/D19-1515.pdf) \\
**Jiacheng Liu**, Julia Hockenmaier \\
EMNLP-IJCNLP 2019 (Oral) \\
[[Arxiv](https://arxiv.org/pdf/1909.00301.pdf)]
[[Code](https://github.com/liujch1998/SoftLabelCCRF)]
[[Slides](https://drive.google.com/file/d/13KSDMj_CdcmwoiNO-gccDG7OHse5ldtw/view?usp=sharing)]

[CrossWeigh: Training Named Entity Tagger from Imperfect Annotations](https://www.aclweb.org/anthology/D19-1519.pdf) \\
Zihan Wang, Jingbo Shang, Liyuan Liu, Lihao Lu, **Jiacheng Liu**, Jiawei Han \\
EMNLP-IJCNLP 2019 (Oral) \\
[[Arxiv](https://arxiv.org/pdf/1909.01441.pdf)]
[[Code](https://github.com/ZihanWangKi/CrossWeigh)]
[[Slides](https://drive.google.com/file/d/1Q_fhl9ksucJPe2UCcdh47saXjPBgK48H/view?usp=sharing)]

---

<br/>
