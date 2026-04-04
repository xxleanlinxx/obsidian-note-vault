# Claude Code Reverse Engineering Knowledge Base

[![Source](https://img.shields.io/badge/Source-Claude%20Code%20v2.1.88-blueviolet?style=flat)](https://github.com/anthropics/claude-code)
[![Notes](https://img.shields.io/badge/Notes-74%20Atomic%20Notes-blue?style=flat)]()
[![Format](https://img.shields.io/badge/Format-Obsidian%20Vault-7C3AED?style=flat&logo=obsidian)](https://obsidian.md/)

> Deep reverse-engineering analysis of **Claude Code v2.1.88** source code (1,884 files / ~92,500 lines of TypeScript), distilled into 74 interconnected atomic notes.

## 📖 About

This knowledge base is a structured, Obsidian-powered analysis of Claude Code's internals. Starting from 75 original analysis reports, the content has been reorganized into atomic notes with extensive cross-references, Mermaid diagrams, and a Map of Content (MOC) navigation layer.

**Scope**: 47 Concept Notes | 10 Pattern Notes | 7 Reference Indexes | 8 MOC Pages

## 🗺️ Knowledge Domains

| Domain | Concepts | Patterns | Key Topics |
|--------|----------|----------|------------|
| Harness Engineering | 7 | 4 | Agent Loop, Context Engineering, Tool Orchestration |
| Prompt Engineering | 4 | 1 | System Prompt Assembly, Compaction, Auxiliary Prompts |
| Tool System | 6 | 2 | 36 Tools, BashTool, Skills vs Tools |
| Agent Architecture | 6 | 1 | Three-Layer Architecture, Coordinator, Swarm |
| Security & Permissions | 5 | 2 | Seven-Layer Defense, AST Parsing, Permission Engine |
| Memory & Context | 7 | 1 | Five Subsystems, Memdir, AutoDream |
| Cost Engineering | 6 | 1 | Cost Tracking, Cache Strategy, Rate Limiting |

## 🔑 Top 10 Key Findings

1. **Agent Loop** is a sophisticated stateful machine
2. **Context Engineering** matters more than Prompt Engineering
3. Security uses a **seven-layer defense-in-depth** model
4. Memory system has **five independent subsystems** coexisting
5. **82 unreleased Feature Flags** reveal the product roadmap
6. **Coordinator / Swarm** are true multi-agent systems
7. **Cost tracking** achieves millisecond-level precision
8. **Skills system** enables programmable prompt extensions
9. Prompt design follows **systematic engineering patterns**
10. The system is deeply designed for **enterprise scenarios**

## 📁 Repository Structure

```
claude-code-leak-note/
├── _MOC/                  # Map of Content (navigation layer)
│   ├── Claude Code 逆向工程知識庫.md   # Home MOC
│   ├── Harness Engineering MOC.md
│   ├── Prompt Engineering MOC.md
│   ├── Tool System MOC.md
│   ├── Agent Architecture MOC.md
│   ├── Security & Permissions MOC.md
│   ├── Memory & Context MOC.md
│   └── Cost Engineering MOC.md
├── concepts/              # 47 atomic concept notes
├── patterns/              # 10 design pattern notes
├── references/            # 7 reference index notes
├── .obsidian/             # Obsidian workspace config
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
   git clone https://github.com/xxleanlinxx/obsidian-note-vault.git
   cd obsidian-note-vault/claude-code-leak-note
   ```

2. **Open in Obsidian**:
   - Launch Obsidian
   - Click "Open folder as vault"
   - Select the `claude-code-leak-note` folder

3. **Start with the Home MOC**:
   - Navigate to `_MOC/Claude Code 逆向工程知識庫.md`
   - This is the main hub linking all domain MOCs

### Usage without Obsidian

Browse the markdown files directly. Note that Obsidian-specific features (like `[[WikiLinks]]` and Mermaid diagrams) may not render correctly in standard Markdown viewers.

## 📐 Cross-Domain Design Patterns

| Pattern | Domains |
|---------|---------|
| Cache Stability Engineering | Cost + Harness |
| Parallel & Async Generator | Harness + Agent |
| Fail-Closed & Deny-First | Security + Harness |
| PII Safe Type System | Harness + Observability |
| Hook Extension System | Tool + Security |

## 📚 Reference Indexes

- **36 Tools Index** — Complete tool catalog with categories
- **6 Built-in Agents Index** — Agent models, toolsets, and purposes
- **16 Bundled Skills Directory** — Built-in skill functions and conditions
- **82 Unreleased Feature Flags** — Hidden feature complete list
- **Codebase Structure Map** — Directory-to-function mapping
- **External References** — Anthropic official, community, and academic resources

## 📝 Note-Taking Methodology

- **Atomic Notes**: Each note covers a single concept or pattern
- **MOC Navigation**: Map of Content pages connect related notes across domains
- **Mermaid Diagrams**: Visual architecture and flow diagrams
- **Cross-References**: Obsidian `[[WikiLinks]]` for seamless navigation
- **Frontmatter Metadata**: `aliases`, `tags`, `created` for search and graph filtering

## 📊 Note Statistics

| Category | Count |
|----------|-------|
| MOC (Navigation) | 8 |
| Concept Notes | 47 |
| Design Pattern Notes | 10 |
| Reference Index Notes | 7 |
| **Total** | **72** |

## 📜 License

This project is licensed under the MIT License.

## 🙏 Acknowledgments

- **Anthropic** — Creator of Claude Code
- The reverse engineering community for analysis methodology

---

**Note**: This is a personal analysis and is not officially affiliated with Anthropic. All original source code belongs to the respective copyright holders.
