# Company News Newsletter Enhanced - Setup Guide

## Overview
This enhanced workflow creates a professional weekly newsletter with company news summaries. It features:
- Individual company research for quality and scale (100+ companies)
- Smart duplicate prevention with week tracking
- Professional HTML email with executive summary and detailed reports
- Google Sheets integration for archival and sharing
- Three-spreadsheet system for organized data management

## Prerequisites
1. n8n instance with the following credentials configured:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
   - Perplexity API

## Setup Instructions

### Step 1: Create the Research Database Spreadsheet
1. Create a new Google Sheet named "Company News Research Database"
2. Add the following headers in row 1:
   - `Company`
   - `Summary`
   - `Links`
   - `WeekNumber`
   - `ISOWeek`
   - `ResearchDate`
   - `EmailSent`
3. Note the spreadsheet ID from the URL (the long string between /d/ and /edit)
4. Share the spreadsheet with your Google service account if using service account auth

### Step 2: Update the Workflow
1. Import the `Company_News_Newsletter_Enhanced.json` file into n8n
2. Find all instances of `RESEARCH_DATABASE_SHEET_ID` and replace with your Research Database spreadsheet ID
3. Verify your Company List spreadsheet ID is correct (should already be set from original workflow)

### Step 3: Configure Credentials
Ensure these credentials are properly configured in n8n:
- **Google Sheets account**: For reading/writing spreadsheets
- **Gmail account**: For sending emails
- **OpenAI account**: For AI processing
- **Perplexity account**: For deep research

### Step 4: Test the Workflow
1. Add a few test companies to your Company List spreadsheet
2. Run the workflow manually using the Manual Trigger
3. Verify:
   - Companies are researched individually
   - Summaries are stored in Research Database with correct week numbers
   - Weekly Summary spreadsheet is created
   - Email is sent with proper formatting
   - Research Database entries are marked as "EmailSent = true"

### Step 5: Set Up Automation (Optional)
Replace the Manual Trigger with a Schedule Trigger:
1. Delete the Manual Trigger node
2. Add a Schedule Trigger node
3. Configure for weekly execution (e.g., Friday 9:00 AM)
4. Connect to "Read Company List" node

## Workflow Architecture

### Three Spreadsheets:
1. **Company List** - Simple list of companies to track
2. **Research Database** - All research data with timestamps and metadata
3. **Weekly Summary** - Clean formatted summary created each week (new sheet)

### Process Flow:
1. **Research Phase**: Each company is researched individually with AI
2. **Storage Phase**: Results stored in Research Database with week tracking
3. **Compilation Phase**: Current week's data compiled into executive summary
4. **Distribution Phase**: Email sent with HTML formatting and spreadsheet link

## Key Features

### Duplicate Prevention
- Uses `ISOWeek` format (YYYY-WW) to track which week summaries belong to
- `EmailSent` boolean prevents re-sending the same summaries
- Only processes summaries where `EmailSent = false` and week matches current

### Scalability
- Handles 100+ companies through individual processing
- No context window limitations
- Can run research incrementally throughout the week

### Email Format
- Professional HTML template with:
  - Executive summary section
  - Quick reference table
  - Detailed company reports
  - Direct link to Google Sheets summary

## Troubleshooting

### No summaries found for current week
- Check that research has been completed for the current week
- Verify week number calculation is correct
- Ensure EmailSent flags are properly set

### Multiple emails sent
- Verify the "Mark Summaries as Sent" node is working
- Check that EmailSent column is being updated in Research Database
- Ensure workflow isn't being triggered multiple times

### Research quality issues
- Adjust the temperature in AI nodes (lower = more consistent)
- Modify system prompts for better guidance
- Check Perplexity API limits

## Advanced Options

### Split into Two Workflows
For better control, split into:
1. **Daily Research Workflow**: Runs multiple times to update company summaries
2. **Weekly Newsletter Workflow**: Compiles and sends the newsletter

### Custom Company Categories
Add a "Category" column to Company List and modify the workflow to:
- Group companies by category in the email
- Create separate sections for different industries
- Generate category-specific insights

### Excel Attachment Option
To attach as Excel instead of Google Sheets link:
1. Add a "Download Google Sheet" node after creating Weekly Summary
2. Convert to Excel format
3. Attach binary data to email using Gmail attachment options

## Maintenance
- Regularly clean old data from Research Database (optional)
- Monitor API usage for OpenAI and Perplexity
- Update company list as needed
- Archive Weekly Summary sheets periodically