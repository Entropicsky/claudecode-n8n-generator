# Company News Newsletter Enhanced v7 - Setup Guide

## What's Fixed in v7 - Company Name Issue Resolved! 🎉

The main issue where all companies showed as "Unknown Company" has been fixed by:

1. **Added Split Out Companies node** - Properly separates each company for individual processing
2. **Store Company Data node** - Stores company info in workflow static data BEFORE the AI Agent runs
3. **Enhanced Format Research Data** - Retrieves company name from static data using the item index

This ensures the company name is preserved through the AI Agent processing and properly appears in all downstream nodes.

## Overview
This enhanced workflow creates a professional weekly newsletter with company news summaries. Each run automatically creates its own Google Sheets database for clean archival.

### Key Features:
- **Company names properly preserved** through AI Agent processing
- **Automatic database creation** - New spreadsheet for each run (no manual setup!)
- **Individual company research** for quality and scale (100+ companies)
- **Professional HTML email** with executive summary and detailed reports
- **Clean separation by week** - Each run has its own database

## Prerequisites
1. n8n instance with the following credentials configured:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
   - Perplexity API (using sonar-pro model)

2. A Google Sheet with your company list

## Setup Instructions

### Step 1: Import the Workflow
1. Import the `Company_News_Newsletter_Enhanced_v7.json` file into n8n
2. The workflow is ready to run - no manual spreadsheet creation needed!

### Step 2: Verify/Update Company List Source
1. The workflow expects a Google Sheet with ID: `1LnQfeGcO80OMRpUJ0fmPaO6Fxeb45N58oh0Qlcz65j4`
2. If your company list is in a different sheet:
   - Open the "Read Company List" node
   - Update the Document ID to your sheet's ID
3. Ensure your sheet has a column named "company" (or "Company") with the company names

### Step 3: Verify Credentials
Ensure these are properly configured in n8n:
- **Google Sheets account**: For reading/writing spreadsheets
- **Gmail account**: For sending emails
- **OpenAI account**: For AI processing (GPT-4o)
- **Perplexity account**: For research (sonar-pro model)

### Step 4: Test the Workflow
1. Add a few test companies to your Company List spreadsheet
2. Run the workflow manually using the Manual Trigger
3. The workflow will:
   - Create a new Research Database (e.g., `Company News Database - 2024-01-15 - Week 2024-03`)
   - Research each company individually
   - Store summaries with correct company names
   - Generate an executive summary
   - Send an HTML email with the full report

## How It Works

### Data Flow Enhancement (v7)
1. **Split Out Companies**: Separates the company array for individual processing
2. **Store Company Data**: Saves company info to workflow static data with unique keys
3. **Company Research Agent**: Processes each company (AI Agent doesn't preserve input data)
4. **Format Research Data**: Retrieves the correct company name from static data

### Complete Workflow Process
1. **Create Database**: Creates new spreadsheet with date/week naming
2. **Setup Headers**: Adds column headers to Research sheet
3. **Read Companies**: Gets list from your source sheet
4. **Store & Research**: Stores company data, then researches each individually
5. **Format & Store**: Formats results with correct company names and saves
6. **Generate Summary**: Creates executive overview
7. **Format & Send**: Sends HTML email with spreadsheet link

## Email Output

The email now correctly shows:
- Company names in the overview table
- Company names in detailed reports
- Proper attribution for each news summary

Example:
```
Company News Overview
┌─────────────┬──────────────────────────────┬───────────┐
│ Company     │ Key Developments             │ Links     │
├─────────────┼──────────────────────────────┼───────────┤
│ Apple       │ Apple announced Q1 results...│ View Details│
│ Microsoft   │ Microsoft unveiled new AI... │ View Details│
└─────────────┴──────────────────────────────┴───────────┘
```

## Troubleshooting

### "Unknown Company" issue (FIXED in v7)
- The workflow now properly stores and retrieves company names
- If you still see "Unknown Company", check that your source sheet has the correct column name

### Spreadsheet not found errors
- All nodes reference the Create Research Database node directly
- Ensure Google Sheets credentials have permission to create files

### Performance with many companies
- The Split Out Companies node processes each company sequentially
- For 100+ companies, expect the workflow to take several minutes
- Consider scheduling during off-peak hours

## Best Practices
1. **Test with few companies first** - Ensure everything works before running on 100+ companies
2. **Monitor API usage** - OpenAI and Perplexity calls can add up
3. **Check company names** - Verify your source sheet has clean company names
4. **Regular schedule** - Weekly runs provide best tracking

## Version History
- **v7**: Fixed company name extraction issue with Split Out and Store Company Data nodes
- **v6**: Used workflow static data to preserve summaries through AI Agent
- **v5**: Attempted merge node approach (had configuration issues)
- **v4**: Used merge nodes for data flow
- **v3**: Fixed pairedItem errors, added proper header setup
- **v2**: Added automatic database creation per run
- **v1**: Original with manual database setup