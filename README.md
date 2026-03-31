# 2025-2026: (10d) Revive & Reproduce CSSL Code:
The Challenges of Continuous Self-Supervised Learning (MinRed)

Optional project of the [Streaming Data Analytics](https://emanueledellavalle.org/teaching/streaming-data-analytics-2025-26/) course provided by [Politecnico di Milano](https://www11.ceda.polimi.it/schedaincarico/schedaincarico/controller/scheda_pubblica/SchedaPublic.do?&evn_default=evento&c_classe=837284&__pj0=0&__pj1=36cd41e96fcd065c47b49d18e46e3110).
co di Milano.

Student: **[To be assigned]**

---

# Brief Description

> **The Challenges of Continuous Self-Supervised Learning**,
> Senthil Purushwalkam et al. (ECCV, 2022)  
> https://github.com/senthilps8/continuous_ssl_problem

The paper proposes the **Minimum Redundancy (MinRed) replay buffer**,
which actively discards the most redundant stored samples based on
feature distance to decorrelate continuous data streams, addressing
catastrophic forgetting and non-IID data challenges in unsupervised
sequential settings.

Your objective is threefold: (1) map the theoretical concepts in the
paper to their actual implementation in the codebase; (2) revive the
public repository (written approximately 3-4 years ago) and make it
run in a modern Python environment; (3) reproduce the simplest
experimental case, adapting the data pipeline to an accessible dataset,
and compare your results against the paper's reported metrics.

> ⚠️ **Note:** The original experiments require large custom datasets
> (Flickr-20M, Kinetics-400). The specific dataset substitute and
> experimental scope will be defined together with the project supervisor.

---

# Background

**Self-Supervised Learning (SSL)** learns visual representations without
human annotations by enforcing invariance to data augmentations (SimCLR,
BYOL, BarlowTwins, SimSiam). **Continual Learning (CL)** studies how
models can learn sequentially without catastrophically forgetting previous
knowledge. Their combination, **Continual Self-Supervised Learning
(CSSL)**, is the focus of this project.

This paper addresses a more realistic CSSL setting where data arrives as
a **continuous, potentially infinite, non-IID stream**. The MinRed buffer
mitigates data correlation by discarding the most redundant stored sample
when a new one arrives. It identifies the most redundant sample $i^*$ as
the one with the minimum cosine distance to its nearest neighbor in the
embedding space:

$$\mathcal{B} \leftarrow \mathcal{B} \setminus \{i^*\} \cup \{x\}
\quad \text{where} \quad
i^* = \arg\min_{i \in \mathcal{B}} \min_{j \in \mathcal{B},\, j \neq i}
d_{\cos}(\bar{z}_i, \bar{z}_j)$$

To ensure stability as the model's weights evolve, buffer representations
are maintained via an exponential moving average:

$$\bar{z}_i = \alpha\bar{z}_i + (1 - \alpha) z_i$$

where $\bar{z}_i$ is the tracked representation of sample $i$, $z_i$ is
its current encoder output, and $\alpha$ is the momentum coefficient.

---

# Project Goals

1. **Map Theory to Code:** Document how the paper's key mechanisms
(buffered SSL training loop, MinRed update rule, feature tracking via
moving average) map to specific files, classes, and functions in the
repository.

2. **Revive the Codebase:** Resolve broken dependencies, deprecated APIs,
and missing configurations. Adapt the data pipeline to work with an
accessible dataset substitute. Document every issue and solution in the
README of your fork.

3. **Reproduce the Baseline Experiment:** SimSiam + Split ImageNet-21K +
4 tasks + Smooth class-incremental. Dataset and scope will be adapted
together with the project supervisor.

4. **Connect to Course Concepts:** Explicitly map the methods and ideas
in the paper to the concepts covered in the course. For each key
mechanism, discuss similarities and differences with respect to what was
presented in the lectures, motivating why the paper's approach converges,
diverges, or extends the theoretical framework seen in class.

---

# Hyperparameters & Dataset

Both will be **defined together with the project supervisor** at the
start of the project and added to this README before experiments begin.
Given the dataset constraints of the original paper, the choice of a
suitable substitute dataset is an integral part of the kickoff discussion.

---

# Evaluation Metrics

Use the standard **linear evaluation** protocol: after training, a frozen
linear classifier is evaluated on top of the learned representations.
Report **Average Accuracy (A)**, **Forgetting (F)**, and **Forward
Transfer (FT)**.

Compare your results across three conditions:
- **Full Training (upper bound):** model trained offline on all data at once.
- **No CL (Conventional SSL baseline):** single-pass training with no buffer.
- **CL technique (MinRed buffer):** the method proposed in the paper.

The experimental setup should follow the simplest configuration described
in the paper, without accounting for computational constraints or
execution time; these will be discussed together with the project
supervisor.

As an optional extension, you may also evaluate representation quality
using **KNN classification** or **CKA similarity** (to measure how much
representations shift across tasks).

Present all results in a single table comparing your values against those
reported in the paper.

---

# Deliverables

1. **GitHub Repository:** Fork of the original codebase with an updated
`requirements.txt` and a README documenting all changes, dataset
adaptations, and instructions to reproduce your experiment.

2. **Jupyter Notebook:** End-to-end pipeline covering data loading,
training, evaluation, and a results table with plots comparing your
metrics against the paper. Inline comments must make explicit which part
of the code corresponds to which mechanism in the paper, and how it
relates to the concepts covered in the course.

3. **Written Report (~1,500 words)** covering:
   - *What you changed and why:* dependencies fixed, dataset substituted,
   configurations adjusted, simplifications made, and the motivation
   behind each choice.
   - *How results compare:* quantitative delta vs. the paper, with a
   reasoned hypothesis for each discrepancy.
   - *Paper-to-code mapping:* for each core contribution (MinRed update
   rule, feature tracking, buffered training loop), identify exactly
   where and how it is implemented in the code.
   - *Connections to the course:* for each key concept in the paper,
   discuss explicitly how it relates to what was covered in the lectures,
   highlighting similarities, differences, and extensions.

---

## Note for Students

* Clone the created repository offline;
* Add your name and surname into the Readme file;
* Make any changes to your repository, according to the specific assignment;
* Add a `requirement.txt` file for code reproducibility and instructions on how to replicate the results;
* Commit your changes to your local repository;
* Push your changes to your online repository.
