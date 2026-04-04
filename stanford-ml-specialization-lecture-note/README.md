# Stanford Machine Learning Specialization - Lecture Notes

[![Course](https://img.shields.io/badge/Coursera-DeepLearning.AI-0056D2?style=flat&logo=coursera)](https://www.coursera.org/specializations/machine-learning-introduction)
[![Instructor](https://img.shields.io/badge/Instructor-Andrew%20Ng-red?style=flat)](https://www.andrewng.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Last Updated](https://img.shields.io/badge/Last%20Updated-2025.03-blue?style=flat)]()

> Comprehensive lecture notes for the Stanford Machine Learning Specialization by Andrew Ng on Coursera (DeepLearning.AI), extended with **post-2020 frontier research** (up to 2025).

## 📖 About

This repository contains detailed lecture notes covering all three courses of the Stanford Machine Learning Specialization. The notes are structured in Markdown format and optimized for use with [Obsidian](https://obsidian.md/), featuring extensive cross-references and a comprehensive knowledge graph.

**Total Duration**: 10 Weeks | **3 Courses** | **10 Weekly Modules** | **11 Knowledge Point Deep-Dives**

## 🎯 Course Overview

### Course 1: Supervised Machine Learning - Regression and Classification

Foundational concepts in supervised learning, including linear regression, logistic regression, and fundamental optimization techniques.

**Topics Covered**:
- Introduction to Machine Learning (supervised/unsupervised learning)
- Linear Regression & Cost Functions
- Gradient Descent & Optimization
- Multiple Feature Regression & Vectorization
- Feature Scaling & Polynomial Regression
- Logistic Regression & Classification
- Decision Boundaries & Regularization

### Course 2: Advanced Learning Algorithms

Deep dive into neural networks, modern optimization techniques, and practical ML engineering.

**Topics Covered**:
- Neural Network Architecture & Forward Propagation
- TensorFlow Implementation
- Activation Functions (ReLU, Softmax)
- Backpropagation & Adam Optimizer
- Bias-Variance Tradeoff
- Train/CV/Test Split Strategies
- Error Analysis & Transfer Learning
- Decision Trees, Random Forests & XGBoost

### Course 3: Unsupervised Learning, Recommenders, Reinforcement Learning

Advanced ML techniques for unsupervised scenarios and sequential decision-making.

**Topics Covered**:
- K-Means Clustering
- Anomaly Detection & Gaussian Density Estimation
- Collaborative Filtering & Content-Based Filtering
- Recommender System Architectures (Two-Tower Networks)
- Principal Component Analysis (PCA)
- Reinforcement Learning Fundamentals
- Markov Decision Processes (MDP)
- Q-Learning & Deep Q-Networks (DQN)

## 📁 Repository Structure

```
Stanford-ML-Specialization-LectureNote/
├── lecture_notes/
│   ├── Course 1 - Supervised Machine Learning/
│   │   ├── C1-W1 - Introduction to Machine Learning.md
│   │   ├── C1-W2 - Regression with Multiple Input Variables.md
│   │   ├── C1-W3 - Classification.md
│   │   └── Course 1 - Index.md
│   ├── Course 2 - Advanced Learning Algorithms/
│   │   ├── C2-W1 - Neural Networks.md
│   │   ├── C2-W2 - Neural Network Training.md
│   │   ├── C2-W3 - Advice for Applying ML.md
│   │   ├── C2-W4 - Decision Trees.md
│   │   └── Course 2 - Index.md
│   ├── Course 3 - Unsupervised Learning, Recommenders, Reinforcement Learning/
│   │   ├── C3-W1 - Clustering & Anomaly Detection.md
│   │   ├── C3-W2 - Recommender Systems & PCA.md
│   │   ├── C3-W3 - Reinforcement Learning.md
│   │   └── Course 3 - Index.md
│   ├── Knowledge Points/
│   │   ├── KP-01 - 超參數與學習率.md
│   │   ├── KP-02 - 現代優化器.md
│   │   ├── KP-03 - 損失函數.md
│   │   ├── KP-04 - 正則化技術.md
│   │   ├── KP-05 - 激活函數.md
│   │   ├── KP-06 - Attention 機制與 Transformer.md
│   │   ├── KP-07 - 縮放法則與湧現能力.md
│   │   ├── KP-08 - 自監督與對比學習.md
│   │   ├── KP-09 - RLHF 與現代強化學習.md
│   │   ├── KP-10 - 現代推薦系統.md
│   │   ├── KP-11 - 表格資料與現代決策樹.md
│   │   └── KP-Index - 知識點總索引.md
│   └── ML Specialization - Master Index.md
├── .obsidian/
│   └── (Obsidian workspace configuration)
├── .gitignore
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- **Recommended**: [Obsidian](https://obsidian.md/) (free, cross-platform note-taking app)
- Alternative: Any Markdown viewer/editor

### Usage with Obsidian

1. **Clone the repository**:
   ```bash
   git clone https://github.com/xxleanlinxx/Stanford-ML-Specialization-LectureNote.git
   cd Stanford-ML-Specialization-LectureNote
   ```

2. **Open in Obsidian**:
   - Launch Obsidian
   - Click "Open folder as vault"
   - Select this repository folder

3. **Start with the Master Index**:
   - Navigate to `lecture_notes/ML Specialization - Master Index.md`
   - This contains the complete knowledge graph and navigation links

### Usage without Obsidian

Simply browse the markdown files in the `lecture_notes/` directory. Note that some Obsidian-specific features (like `[[WikiLinks]]`) may not render correctly in standard Markdown viewers.

## 🗺️ Knowledge Graph

The notes are organized with extensive cross-references following the Map of Content (MOC) methodology:

- **Master Index**: `ML Specialization - Master Index.md` — Complete course overview with Mermaid diagram & KP summary table
- **Course Indexes**: Each course has its own index with weekly breakdown and 延伸知識點 links
- **Knowledge Points (KP)**: 11 deep-dive documents bridging course content with 2020–2025 frontier research
- **KP-Index**: `KP-Index - 知識點總索引.md` — Full knowledge point taxonomy with arxiv paper quick-reference (2020–2025)
- **Cross-References**: Obsidian `[[WikiLinks]]` connect related concepts across courses and KP notes
- **Frontmatter**: All KP files include `aliases` for flexible Obsidian search & graph filtering

## 📊 Key Algorithms Covered

| Algorithm | Type | Use Case |
|-----------|------|----------|
| Linear Regression | Supervised/Regression | Continuous value prediction |
| Logistic Regression | Supervised/Classification | Binary classification |
| Neural Networks | Supervised | Complex tasks (images, text) |
| Decision Trees/XGBoost | Supervised/Ensemble | Tabular data competitions |
| K-Means | Unsupervised/Clustering | Market segmentation |
| Anomaly Detection | Unsupervised | Fraud detection, failure prediction |
| Collaborative Filtering | Unsupervised/Recommendation | Recommender systems |
| PCA | Unsupervised/Dimensionality Reduction | Visualization, compression |
| Deep Q-Network (DQN) | Reinforcement Learning | Game AI, robotics |

## 🔑 Core Concepts Index

### Optimization & Training
- Gradient Descent
- Adam Optimizer
- Backpropagation
- Learning Rate Scheduling

### Model Evaluation
- Bias-Variance Tradeoff
- Cross-Validation
- Precision, Recall & F1 Score
- Error Analysis

### Regularization
- L2 Regularization (Ridge)
- Regularization effects on Bias/Variance
- Dropout & Early Stopping

### Modern Extensions (Post-2020 → 2025)
- Attention Mechanisms & Transformers (MHA → GQA → **MLA** → **NSA**)
- Self-Supervised & Contrastive Learning (SimCLR, CLIP, MAE, DINO → **SigLIP 2**)
- RLHF & Alignment (PPO, DPO → **GRPO**, **DeepSeek-R1**)
- Scaling Laws & Emergent Abilities (Chinchilla → **Test-time Scaling**, **Latent Reasoning**)
- Modern Optimizers (AdamW, Lion → **Sophia**, **Muon**, **SPAM**)
- Normalization Alternatives (**DyT** — Dynamic Tanh replacing LayerNorm)

## 📚 Knowledge Points — Post-2020 Research Extensions

Each Knowledge Point (KP) note supplements the course with rigorously cited research papers (primarily arxiv). **Bold** items denote 2024–2025 additions.

| KP | Topic | Key Concepts | 2024–2025 Additions |
|----|-------|-------------|--------------------|
| KP-01 | Hyperparameters & Learning Rate | Warmup, Cosine Annealing, WSD, μP | — |
| KP-02 | Modern Optimizers | AdamW, Lion, SAM | **Sophia, Schedule-Free, Muon, SPAM** |
| KP-03 | Loss Functions | Label Smoothing, Focal Loss, InfoNCE | — |
| KP-04 | Regularization | LayerNorm, RMSNorm, Mixup, CutMix | **DyT (replacing Normalization)** |
| KP-05 | Activation Functions | GELU, SwiGLU, Mish | — |
| KP-06 | Attention & Transformer | MHA, RoPE, Flash Attention, GQA | **MLA, NSA, DeepSeek-V3** |
| KP-07 | Scaling Laws & Emergence | Kaplan, Chinchilla, CoT, Grokking | **Test-time Scaling, s1, Latent Reasoning** |
| KP-08 | Self-Supervised & Contrastive Learning | SimCLR, CLIP, DINO, MAE | **SigLIP 2** |
| KP-09 | RLHF & Modern RL | PPO, DPO, Constitutional AI | **GRPO, DeepSeek-R1** |
| KP-10 | Modern Recommender Systems | Sequential Rec, DLRM, LLM-Rec | — |
| KP-11 | Tabular Data & Modern Trees | LightGBM, CatBoost, TabNet, SHAP | — |

## 🛠️ Technical Stack Mentioned

- **Frameworks**: TensorFlow, scikit-learn
- **Libraries**: NumPy, Pandas
- **Algorithms**: XGBoost, LightGBM
- **Concepts**: Vectorization, Matrix Computation

## 📝 Note-Taking Methodology

These notes follow best practices for technical learning:
- **Progressive Disclosure**: Start with intuition (白話舉例), then dive into mathematics
- **Cross-References**: Obsidian `[[WikiLinks]]` connect related concepts across courses and KP notes
- **Code Examples**: Include practical implementation notes
- **Visual Aids**: Mermaid diagrams for evolution timelines, architecture comparisons, and concept relationships
- **Knowledge Points**: 11 supplementary documents bridging course content with 2020–2025 frontier research, each with paper tables and arxiv citations
- **Frontmatter Metadata**: Unified `title`, `aliases`, `type`, `category`, `tags`, `related_course_notes`, `related_kp` across all KP files

## 🤝 Contributing

Contributions are welcome! If you find errors or want to add supplementary notes:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (follow Conventional Commits format)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Andrew Ng** - Course Instructor
- **DeepLearning.AI** - Course Provider
- **Coursera** - Platform

## 📬 Contact

For questions or suggestions, please open an issue in this repository.

---

**Note**: These are personal lecture notes and are not officially affiliated with Stanford University, DeepLearning.AI, or Coursera. All course content belongs to the respective copyright holders.
