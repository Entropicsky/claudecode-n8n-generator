# Claude Code n8n Workflow Generator

This repository contains n8n workflow templates generated using Claude Code, demonstrating how to build sophisticated automation workflows with AI assistance.

## 🎯 Featured Workflow: Company News Newsletter Enhanced

An advanced n8n workflow that automatically researches company news and generates professional weekly newsletters with AI-powered summaries.

### Key Features:
- **Automated Company Research**: Uses Perplexity AI to research news for 70+ companies
- **AI-Powered Summaries**: GPT-4o generates executive summaries and formats content
- **Dynamic Email Recipients**: Pulls recipients from Google Sheets
- **Bullet Point Formatting**: Clean, scannable format for easy reading
- **Automatic Database Creation**: Each run creates its own Google Sheets archive
- **Scalable Architecture**: Handles large company lists without hitting API limits

### Version History:
- **v7.3.3** - Production-ready version with all fixes:
  - Handles 70+ companies without Google Sheets limits
  - Dynamic recipient detection
  - Clean email formatting without repetitive overviews
  - Unique timestamps for each run
  
### Prerequisites:
- n8n instance (cloud or self-hosted)
- Google Sheets OAuth2 credentials
- Gmail OAuth2 credentials
- OpenAI API key (GPT-4o)
- Perplexity API key (sonar-pro model)

### Setup:
1. Import the latest workflow JSON into n8n
2. Configure your Google Sheet with:
   - **List** sheet: Company names
   - **Recipients** sheet: Email addresses
3. Update the sheet ID in the workflow nodes
4. Run manually or schedule weekly

## 📁 Repository Structure:
- `Company_News_Newsletter_Enhanced_v*.json` - Workflow versions
- `*_Setup.md` - Setup guides for each version
- `docs/` - Additional documentation and resources
- `CLAUDE.md` - Project-specific Claude Code instructions

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