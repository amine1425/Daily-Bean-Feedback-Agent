# Daily Bean Feedback Agent - Workflow Documentation

## Workflow Overview

This document provides detailed information about the n8n workflow for the Daily Bean Feedback Agent.

## Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         FEEDBACK COLLECTION                              │
│                                                                          │
│  Customer scans QR code → Fills Google Form → Response saved to Sheet   │
└───────────────────────────────┬─────────────────────────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  Google Sheets Trigger│
                    │  (New Row Detection)  │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  AI Feedback Analysis │
                    │  (OpenAI GPT-4)       │
                    │                       │
                    │  Analyzes for:        │
                    │  • Sentiment          │
                    │  • Category           │
                    │  • Priority           │
                    │  • Summary            │
                    │  • Keywords           │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  Parse AI Response    │
                    │  (Extract JSON data)  │
                    └───────────┬───────────┘
                                │
                                ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                          SMART ROUTING LOGIC                              │
└───────────────────────────────────────────────────────────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  Route by Priority    │
                    │  High? OR Negative?   │
                    └───────┬───────┬───────┘
                            │       │
                    YES ────┘       └──── NO
                     │                     │
                     ▼                     ▼
        ┌─────────────────────┐    ┌──────────────────┐
        │  URGENT PATH        │    │  NORMAL PATH     │
        │                     │    │                  │
        │  1. Slack Alert     │    │  Route by        │
        │     (#urgent)       │    │  Category        │
        │                     │    │                  │
        │  2. Customer Email  │    │  ┌─────────────┐ │
        │     (Immediate)     │    │  │  Is Praise? │ │
        │                     │    │  └──┬────────┬─┘ │
        │  3. Log to Sheet    │    │     │ YES    │   │
        └─────────────────────┘    │     ▼        │NO │
                                   │  Slack       │   │
                                   │  (#wins)     ▼   │
                                   │         ┌────────┐│
                                   │         │Is Sug? ││
                                   │         └─┬────┬─┘│
                                   │           │YES │  │
                                   │           ▼    │NO│
                                   │         Slack  │  │
                                   │      (#product)│  │
                                   │                ▼  │
                                   │         Customer  │
                                   │         Email     │
                                   │         (Normal)  │
                                   │                │  │
                                   │                ▼  │
                                   │         Log to    │
                                   │         Sheet     │
                                   └───────────────────┘
```

## Node Details

### 1. Google Sheets Trigger
- **Type**: Trigger Node
- **Function**: Monitors the Google Form response sheet for new entries
- **Polling**: Every minute
- **Output**: New row data with all form fields

### 2. AI Feedback Analysis
- **Type**: OpenAI Node
- **Model**: GPT-4
- **Function**: Analyzes feedback using natural language processing
- **Output**: JSON object with structured analysis

**AI Analysis Output Structure**:
```json
{
  "sentiment": "Positive|Neutral|Negative",
  "category": "Complaint|Suggestion|Praise|Question|Bug Report",
  "priority": "High|Medium|Low",
  "summary": "One sentence summary of the feedback",
  "keywords": ["keyword1", "keyword2", "keyword3"]
}
```

### 3. Parse AI Response
- **Type**: Code Node (JavaScript)
- **Function**: Extracts AI analysis from response and merges with original data
- **Output**: Combined object with original form data + AI analysis

### 4. Route by Priority
- **Type**: IF Node
- **Function**: Determines if feedback requires urgent attention
- **Conditions**: 
  - Priority = "High" OR
  - Sentiment = "Negative"
- **Outputs**: 
  - True → Urgent path
  - False → Normal processing path

### 5. Slack - Urgent Alert
- **Type**: Slack Node
- **Channel**: #customer-feedback-urgent
- **Function**: Sends immediate alert for high-priority feedback
- **Message Format**: Structured with all key details

### 6. Email - Customer Response (Urgent)
- **Type**: Email Send Node
- **Function**: Sends personalized response to customer for urgent matters
- **Template**: Apologizes for issues, confirms receipt, promises quick resolution

### 7. Is Praise?
- **Type**: IF Node
- **Function**: Routes positive feedback to marketing team
- **Condition**: Category = "Praise"

### 8. Slack - Positive Feedback
- **Type**: Slack Node
- **Channel**: #customer-feedback-wins
- **Function**: Shares positive feedback with team, suggests testimonial request

### 9. Is Suggestion?
- **Type**: IF Node
- **Function**: Routes product suggestions to product team
- **Condition**: Category = "Suggestion"

### 10. Slack - Product Team
- **Type**: Slack Node
- **Channel**: #product-ideas
- **Function**: Forwards feature requests and suggestions to product team

### 11. Email - Customer Response (Normal)
- **Type**: Email Send Node
- **Function**: Sends thank you email for non-urgent feedback
- **Template**: Acknowledges feedback, explains next steps

### 12. Log to Processed Sheet
- **Type**: Google Sheets Node
- **Operation**: Append
- **Function**: Logs all processed feedback with AI analysis to tracking sheet
- **Purpose**: Creates searchable database of analyzed feedback

## Routing Logic Summary

| Condition | Route | Actions |
|-----------|-------|---------|
| High Priority OR Negative | Urgent | Slack Alert → Customer Email → Log |
| Praise (Positive) | Marketing | Slack #wins → Customer Email → Log |
| Suggestion | Product | Slack #product → Customer Email → Log |
| Other | Normal | Customer Email → Log |

## Required Credentials

To use this workflow, you need to configure the following credentials in n8n:

1. **Google Sheets OAuth2**
   - For reading form responses and writing to processed sheet
   
2. **OpenAI API**
   - For AI-powered feedback analysis
   - Requires API key
   
3. **Slack API** (Optional)
   - For team notifications
   - Requires workspace integration
   
4. **SMTP Email** (Optional)
   - For customer email responses
   - Can use Gmail, SendGrid, or any SMTP service

## Customization Guide

### Adjusting AI Prompts

Modify the system prompt in the "AI Feedback Analysis" node to:
- Add industry-specific categories
- Change sentiment analysis criteria
- Add custom fields to analyze

### Adding New Routing Rules

1. Add new IF nodes after "Parse AI Response"
2. Define your routing conditions
3. Add corresponding action nodes (Slack, Email, HTTP, etc.)

### Integrating Additional Services

You can add nodes for:
- **Notion**: Create database entries for feedback
- **Airtable**: Log to structured database
- **Discord**: Team notifications
- **HTTP Request**: Webhook to custom services
- **Telegram**: Mobile notifications
- **HubSpot/Salesforce**: CRM updates

### Error Handling

Consider adding error handling nodes:
- Catch errors in AI analysis
- Fallback routing for unrecognized categories
- Notification on workflow failures

## Performance Considerations

- **Polling Frequency**: Default is 1 minute. Adjust based on feedback volume
- **AI Costs**: Each feedback entry makes 1 OpenAI API call
- **Rate Limits**: Consider API rate limits for high-volume scenarios
- **Sheet Size**: Archive old entries periodically to maintain performance

## Testing

Before deploying to production:

1. **Test with sample data**:
   - Submit various types of feedback
   - Verify AI categorization is accurate
   - Check all routing paths work correctly

2. **Verify notifications**:
   - Confirm Slack messages reach correct channels
   - Test email templates with different categories
   - Ensure no duplicate notifications

3. **Check logging**:
   - Verify processed sheet receives all entries
   - Confirm data structure is correct
   - Test sheet permissions

## Monitoring

Key metrics to track:
- Feedback volume per day
- Category distribution
- Average response time
- Customer satisfaction scores
- AI accuracy (spot check)

## Support

For issues or questions about the workflow:
1. Check n8n documentation: https://docs.n8n.io
2. Review OpenAI API documentation for analysis issues
3. Open an issue in this GitHub repository
