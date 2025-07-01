# Company News Newsletter Enhanced

An advanced n8n workflow that automatically researches company news and generates professional weekly newsletters with AI-powered summaries.

## 📁 Folder Structure

- **`main/`** - Contains the latest production-ready version (v7.3.3)
  - `Company_News_Newsletter_Enhanced_v7.3.3.json` - The workflow file
  - `Company_News_Newsletter_Enhanced_v7.3.3_Setup.md` - Setup instructions

- **`archive/`** - Previous versions showing the evolution of the workflow
  - All versions from v1 through v7.3.2
  - Includes intermediate versions that solved specific issues

## 🚀 Quick Start

Use the files in the `main/` folder for production deployment. The archive folder contains previous versions for reference and understanding the development process.

## 🔄 Version History

### Latest (v7.3.3) - Production Ready
- ✅ Handles 70+ companies without Google Sheets limits
- ✅ Dynamic recipient detection from spreadsheet
- ✅ Clean email formatting without repetitive overviews
- ✅ Unique timestamps for each run
- ✅ Resilient error handling

### Key Improvements Through Versions
- **v7.x** - Added dynamic recipients, bullet formatting, fixed company names
- **v6.x** - Simplified data flow with workflow static data
- **v5.x** - Various merge node experiments
- **v4.x** - Attempted different data flow patterns
- **v3.x** - Fixed pairedItem errors, added automatic database creation
- **v2.x** - Three-spreadsheet system improvements
- **v1.x** - Original implementation

## 📋 Features

- **Automated Company Research**: Uses Perplexity AI (sonar-pro) to research news
- **AI-Powered Summaries**: GPT-4o generates executive summaries
- **Dynamic Email Recipients**: Pulls from Google Sheets Recipients tab
- **Bullet Point Formatting**: Clean, scannable format
- **Automatic Database Creation**: Each run creates timestamped archive
- **Scalable Architecture**: Tested with 70+ companies

## 🛠️ Setup

See the setup guide in the `main/` folder for detailed instructions.