# Example Google Form Response Sheet Structure

This document shows what your Google Form response sheet should look like after collecting feedback.

## Sheet Structure

Your Google Sheet should have the following columns (automatically created by Google Forms):

| Timestamp | Name | Email | Feedback Type | Rating | Message |
|-----------|------|-------|---------------|--------|---------|
| 2026-02-17 10:30:45 | John Doe | john@example.com | Praise | 5 | Amazing coffee! Love the new blend. |
| 2026-02-17 11:15:22 | Jane Smith | jane@example.com | Complaint | 2 | Service was slow today. Waited 15 minutes. |
| 2026-02-17 12:00:10 | Bob Wilson | bob@example.com | Suggestion | 4 | Would love to see oat milk as an option! |
| 2026-02-17 13:45:33 | Alice Brown | alice@example.com | Question | 3 | Do you have gluten-free pastries? |

## Column Descriptions

### Timestamp
- **Type**: Date/Time
- **Automatically added by Google Forms**
- **Format**: YYYY-MM-DD HH:MM:SS
- **Used for**: Tracking when feedback was submitted

### Name
- **Type**: Text (Short answer)
- **Required**: Yes
- **Example**: "John Doe"
- **Used for**: Personalizing responses

### Email
- **Type**: Email (Validated)
- **Required**: Yes
- **Example**: "john@example.com"
- **Used for**: Sending automated responses

### Feedback Type
- **Type**: Multiple choice
- **Options**: 
  - Complaint
  - Suggestion
  - Praise
  - Question
  - Other
- **Required**: Yes
- **Used for**: Initial categorization (refined by AI)

### Rating
- **Type**: Linear scale (1-5)
- **Range**: 1 = Very Dissatisfied, 5 = Very Satisfied
- **Required**: Yes
- **Used for**: Quick sentiment indicator

### Message
- **Type**: Paragraph text
- **Required**: Yes
- **Example**: "The coffee was excellent, but the seating area was too crowded."
- **Used for**: Main AI analysis input

## Processed Feedback Sheet

After the workflow processes feedback, it creates a second sheet called "Processed Feedback" with these additional columns:

| Timestamp | Name | Email | Feedback Type | Rating | Message | Sentiment | Category | Priority | Summary | Keywords | Processed At |
|-----------|------|-------|---------------|--------|---------|-----------|----------|----------|---------|----------|--------------|
| 2026-02-17 10:30:45 | John Doe | john@example.com | Praise | 5 | Amazing coffee! Love the new blend. | Positive | Praise | Low | Customer loves new coffee blend | coffee, blend, amazing | 2026-02-17 10:31:12 |
| 2026-02-17 11:15:22 | Jane Smith | jane@example.com | Complaint | 2 | Service was slow today. Waited 15 minutes. | Negative | Complaint | High | Customer experienced slow service with 15 min wait | service, slow, wait | 2026-02-17 11:16:05 |

### Additional AI-Generated Columns

#### Sentiment
- **Values**: Positive, Neutral, Negative
- **Source**: AI Analysis
- **Example**: "Negative"

#### Category
- **Values**: Complaint, Suggestion, Praise, Question, Bug Report
- **Source**: AI Analysis (may differ from user-selected type)
- **Example**: "Complaint"

#### Priority
- **Values**: High, Medium, Low
- **Source**: AI Analysis
- **Example**: "High"

#### Summary
- **Type**: Text (one sentence)
- **Source**: AI Analysis
- **Example**: "Customer experienced slow service with 15 minute wait time"

#### Keywords
- **Type**: Array of strings
- **Source**: AI Analysis
- **Example**: ["service", "slow", "wait", "15 minutes"]

#### Processed At
- **Type**: Timestamp
- **Source**: Workflow execution
- **Example**: "2026-02-17 11:16:05"

## Setting Up Your Sheet

### Initial Setup
1. Create your Google Form with the fields shown above
2. Link to Google Sheet (Forms does this automatically)
3. The first sheet will be named "Form Responses 1"

### For the Workflow
1. Manually create a second sheet in the same spreadsheet
2. Name it "Processed Feedback"
3. The workflow will automatically populate it with headers on first run

### Sheet Permissions
- Ensure your n8n service account has edit access
- For team access, share the sheet with appropriate permissions
- Consider view-only access for most team members

## Sample Test Data

Use these examples to test your workflow:

### Test 1: Positive Feedback
- **Name**: Test User 1
- **Email**: test1@example.com
- **Type**: Praise
- **Rating**: 5
- **Message**: "Absolutely love the atmosphere and the coffee quality. Will definitely come back!"

**Expected Result**: 
- Sentiment: Positive
- Priority: Low
- Route: Marketing team notification

### Test 2: Urgent Complaint
- **Name**: Test User 2
- **Email**: test2@example.com
- **Type**: Complaint
- **Rating**: 1
- **Message**: "Found a hair in my coffee. Very disappointed and will not return."

**Expected Result**:
- Sentiment: Negative
- Priority: High
- Route: Urgent alert to management

### Test 3: Product Suggestion
- **Name**: Test User 3
- **Email**: test3@example.com
- **Type**: Suggestion
- **Rating**: 4
- **Message**: "Great place! It would be even better with more vegan food options."

**Expected Result**:
- Sentiment: Positive
- Category: Suggestion
- Priority: Medium
- Route: Product team channel

### Test 4: General Question
- **Name**: Test User 4
- **Email**: test4@example.com
- **Type**: Question
- **Rating**: 3
- **Message**: "Do you offer catering for corporate events? Need info for planning."

**Expected Result**:
- Sentiment: Neutral
- Category: Question
- Priority: Medium
- Route: Customer email response

## Data Management Tips

### Regular Maintenance
- Archive old responses monthly (move to separate sheet)
- Keep active sheet under 1000 rows for performance
- Backup data regularly

### Analytics
- Use pivot tables to analyze trends
- Track sentiment over time
- Monitor category distribution
- Calculate average response time

### Privacy Considerations
- Customer emails are PII - handle appropriately
- Consider data retention policies
- Comply with GDPR/privacy regulations
- Anonymize data for public reporting

## Customization

You can add more fields to your form:
- **Phone number** (for urgent follow-ups)
- **Location** (if you have multiple sites)
- **Order number** (to link feedback to transactions)
- **Visit frequency** (First time, Regular, Occasional)
- **Age group** (demographics)
- **How did you hear about us?** (marketing attribution)

Remember to update the AI prompt and workflow nodes if you add new fields.
