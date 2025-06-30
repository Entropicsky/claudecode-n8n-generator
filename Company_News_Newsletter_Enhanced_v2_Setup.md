# Company News Newsletter Enhanced v2 - Setup Guide

## Overview
This enhanced workflow creates a professional weekly newsletter with company news summaries. The key improvement in v2 is that it automatically creates a new Research Database spreadsheet for each run, providing a clean archive system.

### Key Features:
- **Automatic database creation** - New spreadsheet for each run (no manual setup!)
- **Individual company research** for quality and scale (100+ companies)
- **Professional HTML email** with executive summary and detailed reports
- **Google Sheets integration** for archival and sharing
- **Clean separation by week** - Each run has its own database

## Prerequisites
1. n8n instance with the following credentials configured:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
   - Perplexity API

2. A Google Sheet with your company list (the workflow expects the default ID, but you can update it)

## Setup Instructions

### Step 1: Import the Workflow
1. Import the `Company_News_Newsletter_Enhanced_v2.json` file into n8n
2. The workflow is ready to run - no manual spreadsheet creation needed!

### Step 2: Verify/Update Company List Source
1. The workflow expects a Google Sheet with ID: `1LnQfeGcO80OMRpUJ0fmPaO6Fxeb45N58oh0Qlcz65j4`
2. If your company list is in a different sheet:
   - Open the "Read Company List" node
   - Update the Document ID to your sheet's ID
3. Ensure your sheet has a column named "company" with the company names

### Step 3: Configure Credentials
Ensure these credentials are properly configured in n8n:
- **Google Sheets account**: For reading/writing spreadsheets
- **Gmail account**: For sending emails
- **OpenAI account**: For AI processing
- **Perplexity account**: For deep research

### Step 4: Test the Workflow
1. Add a few test companies to your Company List spreadsheet
2. Run the workflow manually using the Manual Trigger
3. The workflow will:
   - Create a new Research Database named with today's date and week number
   - Research each company individually
   - Store summaries in the Research sheet
   - Generate an executive summary
   - Populate the Weekly Summary sheet
   - Send an HTML email with the full report

## How It Works

### Automatic Database Creation
Each run creates a new Google Spreadsheet named:
- `Company News Database - YYYY-MM-DD - Week YYYY-WW`
- Example: `Company News Database - 2024-01-15 - Week 2024-03`

This spreadsheet contains two sheets:
1. **Research** - Raw research data for each company
2. **Weekly Summary** - Formatted summary table

### Workflow Process
1. **Create Database**: Creates new spreadsheet with proper naming
2. **Store ID**: Saves spreadsheet ID in workflow memory
3. **Read Companies**: Gets list from your source sheet
4. **Research Loop**: Each company researched individually
5. **Store Results**: Saves to the Research sheet
6. **Generate Summary**: Creates executive overview
7. **Format & Send**: Sends HTML email with link to full report

### Benefits of Per-Run Databases
- **Clean Archives**: Each week's research is preserved separately
- **No Contamination**: Previous weeks don't affect current run
- **Easy Review**: Can access any past newsletter by date
- **No Manual Cleanup**: No need to clear old data

## Email Format
The email includes:
- Professional HTML formatting
- Executive summary section
- Quick reference table (Company | Key Developments | Links)
- Detailed reports for each company
- Direct link to the Google Sheets database

## Automation Options

### Weekly Schedule
Replace the Manual Trigger with a Schedule Trigger:
1. Delete the Manual Trigger node
2. Add a Schedule Trigger node
3. Configure for weekly execution (e.g., Friday 9:00 AM)
4. Connect to "Create Research Database" node

### Split Workflow Option
For more control, you could split into:
1. **Research Workflow**: Runs throughout the week, updating a shared database
2. **Newsletter Workflow**: Compiles and sends weekly

However, the current design's simplicity (new database per run) is recommended for most use cases.

## Troubleshooting

### Workflow fails at "Create Research Database"
- Check Google Sheets credentials have permission to create new files
- Verify you're not hitting Google API quotas

### No companies found
- Verify the Company List spreadsheet ID is correct
- Check that the column is named "company" (lowercase)
- Ensure there are no empty rows at the top

### Research quality issues
- Adjust the temperature in AI nodes (lower = more consistent)
- Modify system prompts for better guidance
- Check Perplexity API limits

### Email not sending
- Verify Gmail credentials are properly configured
- Check that the email address in the workflow is correct
- Ensure Gmail API is enabled in Google Cloud Console

## Advanced Customization

### Custom Naming Convention
To change how databases are named, edit the "Create Research Database" node:
- Current: `Company News Database - {{ DateTime.local().toFormat('yyyy-MM-dd') }} - Week {{ DateTime.local().toFormat('kkkk-WW') }}`
- Monthly: `Company News Database - {{ DateTime.local().toFormat('yyyy-MM') }}`
- Custom: Add any n8n expression for dynamic naming

### Add More Metadata
To track additional information per company:
1. Edit the "Create Research Database" node to add more column headers
2. Update "Format Research Data" to include new fields
3. Modify email template to display new data

### Excel Export Option
To also save as Excel:
1. Add a "Google Sheets Download" node after populating
2. Convert to Excel format
3. Can email as attachment or save to cloud storage

## Maintenance
- **No regular maintenance needed** - each run is self-contained
- Periodically review old databases and delete if not needed
- Monitor API usage for OpenAI and Perplexity
- Update company list as needed

## Best Practices
1. **Run consistently** - Weekly runs provide best tracking
2. **Review archives** - Past databases show trends over time
3. **Share selectively** - Each database can be shared independently
4. **Monitor costs** - AI API calls can add up with many companies