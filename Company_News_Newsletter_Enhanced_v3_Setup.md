# Company News Newsletter Enhanced v3 - Setup Guide

## What's Fixed in v3
1. **Proper Spreadsheet ID References**: All nodes now use direct references to `$('Create Research Database').item.json.spreadsheetId`
2. **Column Headers Setup**: Added dedicated nodes to set up Research sheet headers before storing data
3. **Simplified Architecture**: Removed unnecessary workflow static data storage

## Overview
This enhanced workflow creates a professional weekly newsletter with company news summaries. Each run automatically creates its own Google Sheets database for clean archival.

### Key Features:
- **Automatic database creation** - New spreadsheet for each run (no manual setup!)
- **Proper header initialization** - Sets up columns before data storage
- **Individual company research** for quality and scale (100+ companies)
- **Professional HTML email** with executive summary and detailed reports
- **Direct node references** - No more pairedItem errors
- **Clean separation by week** - Each run has its own database

## Prerequisites
1. n8n instance with the following credentials configured:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
   - Perplexity API

2. A Google Sheet with your company list

## Setup Instructions

### Step 1: Import the Workflow
1. Import the `Company_News_Newsletter_Enhanced_v3.json` file into n8n
2. The workflow is ready to run - no manual spreadsheet creation needed!

### Step 2: Verify/Update Company List Source
1. The workflow expects a Google Sheet with ID: `1LnQfeGcO80OMRpUJ0fmPaO6Fxeb45N58oh0Qlcz65j4`
2. If your company list is in a different sheet:
   - Open the "Read Company List" node
   - Update the Document ID to your sheet's ID
3. Ensure your sheet has a column named "company" with the company names

### Step 3: Verify Credentials
Ensure these are properly configured in n8n:
- **Google Sheets account**: For reading/writing spreadsheets
- **Gmail account**: For sending emails
- **OpenAI account**: For AI processing
- **Perplexity account**: For deep research

### Step 4: Test the Workflow
1. Add a few test companies to your Company List spreadsheet
2. Run the workflow manually using the Manual Trigger
3. The workflow will:
   - Create a new Research Database (e.g., `Company News Database - 2024-01-15 - Week 2024-03`)
   - Set up the Research sheet with proper headers
   - Research each company individually
   - Store summaries in the Research sheet
   - Generate an executive summary
   - Populate the Weekly Summary sheet
   - Send an HTML email with the full report

## How It Works

### Workflow Process
1. **Create Database**: Creates new spreadsheet with date/week naming
2. **Setup Headers**: Adds column headers to Research sheet
3. **Read Companies**: Gets list from your source sheet
4. **Research Loop**: Each company researched individually
5. **Store Results**: Saves to Research sheet with all metadata
6. **Generate Summary**: Creates executive overview
7. **Format & Send**: Sends HTML email with spreadsheet link

### Database Structure
Each run creates a spreadsheet with two sheets:
1. **Research Sheet**
   - Company
   - Summary
   - Links
   - WeekNumber
   - ISOWeek
   - ResearchDate
   - EmailSent

2. **Weekly Summary Sheet**
   - Company
   - Key Developments
   - Impact/Significance
   - Related Links
   - Research Date

## Troubleshooting

### "Paired item data unavailable" error
This has been fixed in v3 by:
- Using direct node references instead of passed data
- Properly returning arrays from Code nodes
- Including pairedItem references where needed

### Spreadsheet not found errors
- All nodes now reference the Create Research Database node directly
- Check that your Google Sheets credentials have permission to create files

### No companies found
- Verify the Company List spreadsheet ID is correct
- Check that the column is named "company" (lowercase)
- Ensure there are no empty rows at the top

### Headers not showing in spreadsheet
- The workflow now explicitly creates headers before adding data
- If issues persist, check the "Setup Research Headers" node

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

## Advanced Customization

### Custom Database Naming
Edit the "Create Research Database" node title field:
- Current: `Company News Database - {{ DateTime.local().toFormat('yyyy-MM-dd') }} - Week {{ DateTime.local().toFormat('kkkk-WW') }}`
- Monthly: `Company News Database - {{ DateTime.local().toFormat('yyyy-MM') }}`
- Quarterly: `Company News Database - {{ DateTime.local().toFormat('yyyy') }} Q{{ DateTime.local().quarter }}`

### Add More Columns
To track additional information:
1. Edit "Prepare Headers" node to add new column names
2. Update "Format Research Data" to include new fields
3. Modify email template if you want to display new data

### Different Email Recipients
Edit the "Format Email" node to change the recipient or add multiple recipients

## Best Practices
1. **Test with few companies first** - Ensure everything works before running on 100+ companies
2. **Monitor API usage** - OpenAI and Perplexity calls can add up
3. **Regular schedule** - Weekly runs provide best tracking
4. **Archive old databases** - Periodically review and clean up old spreadsheets

## Version History
- **v3**: Fixed pairedItem errors, added proper header setup, direct node references
- **v2**: Added automatic database creation per run
- **v1**: Original with manual database setup