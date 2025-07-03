# Supplier Part Research Agent Setup Guide

## Overview
This workflow performs automated research on supplier parts, analyzing customer sentiment and feedback from the last 90 days. It processes up to 1000 parts, generates sentiment scores (0-10), and alerts you via Slack for problematic products.

## Prerequisites

### Required Credentials
1. **Google Sheets OAuth2** - For reading input data and writing results
2. **Perplexity API** - For deep research capabilities
3. **OpenAI API** - For sentiment analysis
4. **Slack Webhook** - For sending alerts

### Input Google Sheet Format
Your input sheet must have these exact column names:
- `Supplier Name` - The manufacturer/supplier name
- `Part Number` - The product part number
- `Description` - Product description

## Setup Instructions

### Step 1: Import the Workflow
1. Open n8n and go to Workflows
2. Click "Import" and select the JSON file
3. Save the workflow

### Step 2: Configure Credentials

#### Google Sheets
1. Click on "Read Supplier Sheet" node
2. Click "Create New Credential"
3. Follow Google's OAuth2 flow
4. Grant access to Google Sheets
5. Repeat for "Write Results Sheet" node

#### Perplexity API
1. Get your API key from https://www.perplexity.ai/settings/api
2. Click on "Perplexity Model" node
3. Add new credential with your API key

#### OpenAI API
1. Get your API key from https://platform.openai.com/api-keys
2. Click on "OpenAI Sentiment Model" node
3. Add new credential with your API key

#### Slack Webhook
1. Go to https://api.slack.com/apps
2. Create a new app or use existing
3. Enable Incoming Webhooks
4. Add new webhook to your desired channel
5. Copy the webhook URL
6. Add to "Send Slack Alert" node

### Step 3: Configure Sheet Settings

#### Input Sheet
1. Open "Read Supplier Sheet" node
2. Select your Google Sheet from the dropdown
3. Or paste the sheet URL directly

#### Output Sheet
1. Open "Write Results Sheet" node
2. Select the same or different Google Sheet
3. The workflow creates a new sheet with timestamp

### Step 4: Test the Workflow
1. Start with a small test sheet (5-10 parts)
2. Click "Execute Workflow" 
3. Monitor the execution
4. Check output sheet and Slack alerts

## Workflow Features

### Batch Processing
- Processes parts in batches of 10
- Prevents API rate limit issues
- Handles up to 1000 parts efficiently

### Deep Research
The Perplexity agent searches:
- Reddit discussions
- AV forums (AVSForum, AVForums)
- Retailer reviews (Amazon, B&H, Crutchfield)
- Tech news sites
- Social media mentions
- Industry publications

### Sentiment Scoring (0-10)
- **9-10**: Overwhelmingly positive, highly recommended
- **7-8**: Mostly positive with minor issues
- **5-6**: Mixed feedback
- **3-4**: Significant problems, frequent complaints
- **0-2**: Major issues, recalls, widespread dissatisfaction

### Scoring Weights
- Customer complaints: 40%
- Verified purchase reviews: 30%
- Technical/professional reviews: 20%
- General comments/social media: 10%

### Output Columns
- Part Number
- Supplier
- Description
- Sentiment Score
- Summary
- Key Issues
- Sources
- Research Date
- Full Research (detailed findings)

### Slack Alerts
Automatic alerts for parts scoring 4 or below include:
- Supplier and part number
- Sentiment score
- Summary of issues
- Key problems identified

## Troubleshooting

### Common Issues

1. **Google Sheets Access Error**
   - Ensure sheet is shared with the service account
   - Check OAuth permissions include Sheets access

2. **Perplexity Rate Limits**
   - Reduce batch size if hitting limits
   - Add delay between batches

3. **No Slack Alerts**
   - Verify webhook URL is correct
   - Check Slack channel permissions
   - Ensure parts are scoring ≤4

4. **Timeout Errors**
   - For 1000+ parts, consider running in smaller chunks
   - Increase workflow timeout in settings

### Performance Tips
- Run during off-peak hours for better API performance
- Monitor API usage to stay within limits
- Archive old result sheets regularly

## Customization Options

### Adjust Batch Size
In "Process in Batches" node, change `batchSize` (default: 10)

### Modify Research Timeframe
In "Perplexity Research Agent" system message, change "last 90 days"

### Change Sentiment Threshold
In "Check for Alerts" node, modify the condition value (default: 4)

### Add Additional Columns
Modify "Format Results" node to include extra data fields

## Support
For issues or questions:
- Check n8n documentation
- Review error messages in execution logs
- Verify all credentials are active