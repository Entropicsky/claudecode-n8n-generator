# Claude Code n8n Workflow Generator

This repository contains n8n workflow templates generated using Claude Code, demonstrating how to build sophisticated automation workflows with AI assistance.

## 📁 Repository Structure

```
workflows/
├── company-news-newsletter/
│   ├── main/              # Latest production version
│   ├── archive/           # Previous versions
│   └── README.md          # Workflow documentation
└── [future-workflows]/    # Additional workflows follow same structure

docs/                      # Documentation and resources
├── n8nagents.md          # AI Agent guidelines
├── n8n-templates-index.md # Template reference
└── awesome-n8n-templates/ # Template examples

CLAUDE.md                  # Claude Code instructions for workflow creation
```

## 🎯 Featured Workflows

### Company News Newsletter Enhanced
An advanced n8n workflow that automatically researches company news and generates professional weekly newsletters with AI-powered summaries.

**Location**: `workflows/company-news-newsletter/`

**Key Features**:
- Automated research for 70+ companies using Perplexity AI
- AI-powered executive summaries with GPT-4o
- Dynamic email recipients from Google Sheets
- Clean bullet-point formatting
- Automatic database creation per run
- Production-tested and scalable

**Latest Version**: v7.3.3 (see `main/` folder)

## 🚀 Getting Started

1. **Browse workflows** in the `workflows/` directory
2. **Use files from `main/`** folders for production
3. **Check `archive/`** folders to understand development history
4. **Follow setup guides** included with each workflow

## 📋 Workflow Development Standards

All workflows in this repository follow consistent organization:
- **main/**: Latest stable version ready for production
- **archive/**: Previous versions showing evolution
- **README.md**: Workflow-specific documentation
- **Semantic versioning**: Clear version progression

## 🛠️ Prerequisites

Common requirements across workflows:
- n8n instance (cloud or self-hosted)
- API credentials as specified in each workflow
- Basic understanding of n8n node configuration

## 🤖 About Claude Code Generation:
These workflows were created through iterative development with Claude Code, demonstrating:
- Complex data flow management
- AI agent integration
- Error handling and resilience
- Performance optimization for scale

## 🚀 Getting Started:
1. Choose the latest workflow version (v7.3.3 recommended)
2. Import into your n8n instance
3. Follow the setup guide for configuration
4. Test with a small batch before full deployment

## 📝 License:
MIT - Feel free to use and modify these workflows for your needs.

## 🤝 Contributing:
Contributions welcome! Please submit issues or PRs for improvements.

---
Generated with [Claude Code](https://claude.ai/code)