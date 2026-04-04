# Obsidian Note Vault

[![Format](https://img.shields.io/badge/Format-Obsidian%20Vault-7C3AED?style=flat&logo=obsidian)](https://obsidian.md/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> A collection of interconnected Obsidian knowledge bases covering machine learning fundamentals, frontier research, and AI agent engineering.

## 📖 About

This repository hosts multiple Obsidian vaults, each focusing on a distinct technical domain. Notes are written in Markdown with extensive `[[WikiLinks]]`, Mermaid diagrams, and Map of Content (MOC) navigation for a rich knowledge-graph experience.

## 📚 Knowledge Bases

| Project | Topic | Notes | Description |
|---------|-------|-------|-------------|
| [stanford-ml-specialization-lecture-note](./stanford-ml-specialization-lecture-note/) | Machine Learning | 32 | Stanford ML Specialization (Andrew Ng) lecture notes + 11 post-2020 frontier research deep-dives |
| [claude-code-leak-note](./claude-code-leak-note/) | AI Agent Engineering | 72 | Deep reverse-engineering analysis of Claude Code v2.1.88 (~92,500 lines TypeScript) |

## 🚀 Getting Started

### Prerequisites

- **Recommended**: [Obsidian](https://obsidian.md/) (free, cross-platform)
- Alternative: Any Markdown viewer/editor

### Quick Start

1. **Clone the repository**:
   ```bash
   git clone https://github.com/xxleanlinxx/obsidian-note-vault.git
   cd obsidian-note-vault
   ```

2. **Open a vault in Obsidian**:
   - Launch Obsidian → "Open folder as vault"
   - Select either `stanford-ml-specialization-lecture-note/` or `claude-code-leak-note/`

3. **Navigate via MOC**:
   - Each vault has a `_MOC/` folder with Map of Content pages as the main entry point

### Note

Obsidian-specific features such as `[[WikiLinks]]` and Mermaid diagrams may not render correctly in standard Markdown viewers or on GitHub. For the best experience, open the vaults in Obsidian.

## 🗺️ Vault Highlights

### Stanford ML Specialization Lecture Notes

- **3 courses** × 10 weekly modules covering supervised/unsupervised learning, recommenders, and reinforcement learning
- **11 Knowledge Point** notes bridging course content with 2020–2025 frontier research (Transformers, RLHF, Scaling Laws, etc.)
- Extensive formula references, algorithm comparison tables, and arxiv paper citations

### Claude Code Reverse Engineering

- **7 domain MOCs**: Harness Engineering, Prompt Engineering, Tool System, Agent Architecture, Security & Permissions, Memory & Context, Cost Engineering
- **47 concept notes**, **10 design pattern notes**, **7 reference indexes**
- Covers Agent Loop, multi-agent coordination, seven-layer security model, and more

## 📜 License

This project is licensed under the MIT License.

---

**Note**: These are personal study and analysis notes. They are not officially affiliated with Stanford University, DeepLearning.AI, Coursera, or Anthropic. All original content belongs to the respective copyright holders.