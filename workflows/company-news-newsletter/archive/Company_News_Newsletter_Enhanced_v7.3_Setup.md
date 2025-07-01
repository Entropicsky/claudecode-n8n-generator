# Company News Newsletter Enhanced v7.3 - Setup Guide

## What's New in v7.3 - Better Formatting with Bullet Points! 📋

The newsletter now uses structured bullet points for much better readability:

### Executive Summary:
- **Market Overview**: Key trends with bullet points
- **Notable Developments**: Company-specific achievements
- **Financial Highlights**: Metrics and numbers
- **Strategic Implications**: Forward-looking insights

### Company Research:
- **Overview**: Brief 1-2 sentence summary
- **Key Developments**: Date-stamped bullet points
- **Financial/Business Impact**: Quantified metrics
- **Sources**: Clean link list

This makes the email much easier to scan and reduces unnecessary fluff!

## Previous Updates
- **v7.2**: Dynamic email recipients from Recipients sheet
- **v7.1**: Fixed Split Out node issue
- **v7.0**: Fixed company name preservation through AI Agent

## Overview
This enhanced workflow creates a professional weekly newsletter with company news summaries. Each run automatically creates its own Google Sheets database for clean archival.

### Key Features:
- **Bullet point formatting** for easy scanning
- **Company names properly preserved** through AI Agent processing
- **Automatic database creation** - New spreadsheet for each run
- **Individual company research** for quality and scale (100+ companies)
- **Dynamic email recipients** from spreadsheet
- **Professional HTML email** with executive summary and detailed reports

## Prerequisites
1. n8n instance with the following credentials configured:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
   - Perplexity API (using sonar-pro model)

2. A Google Sheet with:
   - Company list (in "List" sheet)
   - Email recipients (in "Recipients" sheet)

## Setup Instructions

### Step 1: Import the Workflow
1. Import the `Company_News_Newsletter_Enhanced_v7.3.json` file into n8n
2. The workflow is ready to run - no manual spreadsheet creation needed!

### Step 2: Configure Your Google Sheet
The workflow expects a Google Sheet with ID: `1LnQfeGcO80OMRpUJ0fmPaO6Fxeb45N58oh0Qlcz65j4`

This sheet should have two tabs:
1. **List** - Contains company names in a column named "company" (or "Company")
2. **Recipients** - Contains email addresses in a column named "email" (or "Email")

If your sheet has a different ID:
- Open both "Read Company List" and "Read Email Recipients" nodes
- Update the Document ID to your sheet's ID

### Step 3: Verify Credentials
Ensure these are properly configured in n8n:
- **Google Sheets account**: For reading/writing spreadsheets
- **Gmail account**: For sending emails
- **OpenAI account**: For AI processing (GPT-4o)
- **Perplexity account**: For research (sonar-pro model)

### Step 4: Test the Workflow
1. Add a few test companies to your List sheet
2. Add test email recipients to your Recipients sheet
3. Run the workflow manually using the Manual Trigger
4. The workflow will:
   - Create a new Research Database
   - Research each company with bullet-point format
   - Generate executive summary with structured sections
   - Send HTML email to all recipients

## Sample Output Format

### Executive Summary:
**Market Overview:**
• Tech sector shows 15% growth across tracked companies
• Multiple partnership announcements signal industry consolidation
• Q4 earnings exceed analyst expectations by average 8%

**Notable Developments:**
• Apple - Launched Vision Pro 2 with 40% lighter design
• Microsoft - $10B investment in quantum computing research
• Google - DeepMind achieves breakthrough in protein folding

### Company Research:
**Overview:** Apple announced strong Q4 results with record iPhone sales.

**Key Developments:**
• [Oct 26] - Q4 revenue up 12% YoY to $94.9B
• [Oct 28] - Vision Pro 2 launch with improved battery life
• [Oct 30] - New M4 chip announced for MacBook Pro

**Financial/Business Impact:**
• Revenue: $94.9B (+12% YoY)
• iPhone sales: Up 8% globally
• Services revenue: Record $22.3B

## Email Formatting

The HTML email now includes:
- Properly formatted bullet points with CSS styling
- Clear section headers in bold
- Structured lists for easy scanning
- Consistent spacing and indentation
- Professional color scheme

## Troubleshooting

### Bullet points not displaying correctly
- The workflow converts markdown formatting to HTML
- Check that the AI Agents are using the bullet character (•)
- Verify the Format Email node's HTML conversion function

### Recipients not found
- Ensure the Recipients sheet has an "email" or "Email" column
- Check that email addresses are valid
- Remove any empty rows in the Recipients sheet

### Company research too verbose
- The AI Agent system prompts enforce bullet point format
- If needed, adjust the maxTokens parameter in the Research LLM node

## Best Practices
1. **Keep recipient list updated** - Add/remove emails from Recipients sheet as needed
2. **Test formatting** - Run with 2-3 companies first to verify bullet point formatting
3. **Monitor length** - Bullet points should be concise (one key point each)
4. **Regular schedule** - Weekly runs provide best tracking

## Version History
- **v7.3**: Added bullet point formatting for better readability
- **v7.2**: Dynamic email recipients from Recipients sheet
- **v7.1**: Fixed Split Out node issue
- **v7.0**: Fixed company name extraction with workflow static data
- **v6**: Used workflow static data to preserve summaries
- **v5-v4**: Various merge node attempts
- **v3**: Fixed pairedItem errors
- **v2**: Added automatic database creation
- **v1**: Original with manual setup