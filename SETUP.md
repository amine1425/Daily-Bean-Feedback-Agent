# Quick Setup Guide

This guide will help you set up the Daily Bean Feedback Agent in 30 minutes or less.

## Step 1: Create Your Google Form (5 minutes)

1. Go to [Google Forms](https://forms.google.com)
2. Create a new form with these fields:
   - **Name** (Short answer)
   - **Email** (Short answer, validated)
   - **Feedback Type** (Multiple choice: Complaint, Suggestion, Praise, Question, Other)
   - **Rating** (Linear scale: 1-5)
   - **Message** (Paragraph)

3. Configure form settings:
   - ✓ Collect email addresses
   - ✓ Limit to 1 response
   - Set confirmation message: "Thank you! We'll review your feedback shortly."

4. Link to Google Sheets:
   - Click "Responses" tab
   - Click the green Sheets icon
   - Create a new spreadsheet

5. Create a QR code:
   - Copy your form URL
   - Use [QR Code Generator](https://www.qr-code-generator.com/) to create a QR code
   - Download and print for your location

## Step 2: Set Up n8n (10 minutes)

### Option A: n8n Cloud (Easiest)
1. Sign up at [n8n.cloud](https://n8n.cloud)
2. Choose a plan (free tier available)

### Option B: Self-Hosted
1. Install n8n:
   ```bash
   npm install n8n -g
   n8n start
   ```
2. Access n8n at `http://localhost:5678`

## Step 3: Configure Credentials (5 minutes)

### Google Sheets
1. In n8n, go to **Credentials** → **New**
2. Select **Google Sheets OAuth2 API**
3. Follow the authorization flow
4. Name it "Google Sheets Account"

### OpenAI
1. Get API key from [OpenAI Platform](https://platform.openai.com/api-keys)
2. In n8n, go to **Credentials** → **New**
3. Select **OpenAI API**
4. Paste your API key
5. Name it "OpenAI Account"

### Slack (Optional but Recommended)
1. Create a Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Add OAuth scopes: `chat:write`, `channels:read`
3. Install to your workspace
4. Copy the Bot User OAuth Token
5. In n8n, add Slack credential with the token

### Email (Optional)
1. Use your existing SMTP service (Gmail, SendGrid, etc.)
2. For Gmail:
   - Enable 2-factor authentication
   - Create an [App Password](https://support.google.com/accounts/answer/185833)
3. In n8n, add SMTP credentials

## Step 4: Import the Workflow (2 minutes)

1. In n8n, go to **Workflows** → **Import**
2. Click **Upload** and select `daily-bean-feedback-workflow.json`
3. Click **Import**

## Step 5: Configure the Workflow (5 minutes)

### Update Google Sheets Trigger
1. Click on "Google Sheets Trigger" node
2. Select your credentials
3. Choose the spreadsheet (your form responses)
4. Select sheet name: "Form Responses 1"
5. Save

### Update AI Analysis Node
1. Click on "AI Feedback Analysis" node
2. Select your OpenAI credentials
3. Verify the prompt is appropriate for your use case
4. Save

### Update Slack Nodes (if using)
1. For each Slack node:
   - Select your Slack credentials
   - Choose or create the channel
   - Customize message templates if desired
   - Save

### Update Email Nodes (if using)
1. For each Email node:
   - Select your SMTP credentials
   - Update "From Email" address
   - Customize email templates
   - Save

### Update Processed Sheet Logger
1. Click on "Log to Processed Sheet" node
2. Select your Google Sheets credentials
3. Choose the same spreadsheet
4. Create a new sheet called "Processed Feedback"
5. Save

## Step 6: Test the Workflow (3 minutes)

1. **Activate** the workflow (toggle in top right)

2. **Submit a test feedback**:
   - Scan your QR code or visit the form URL
   - Fill out with test data
   - Submit

3. **Monitor execution**:
   - In n8n, go to **Executions**
   - Watch the workflow run
   - Verify all nodes execute successfully

4. **Check outputs**:
   - ✓ Slack messages sent (if configured)
   - ✓ Email received (check spam folder)
   - ✓ Entry logged to "Processed Feedback" sheet

## Troubleshooting

### Workflow doesn't trigger
- Check polling interval (default: 1 minute)
- Verify Google Sheets credentials are valid
- Ensure sheet name matches exactly

### AI analysis fails
- Verify OpenAI API key is correct
- Check you have credits in your OpenAI account
- Review API rate limits

### Slack messages not sending
- Verify bot has access to the channel
- Check OAuth scopes include `chat:write`
- Invite the bot to private channels

### Emails not sending
- Check SMTP credentials
- Verify sender email is authorized
- Check recipient's spam folder

## Cost Estimates

- **n8n Cloud**: Free tier available, paid plans from $20/month
- **n8n Self-hosted**: Free (hosting costs only)
- **OpenAI API**: ~$0.01-0.03 per feedback analysis (GPT-4)
- **Google Forms/Sheets**: Free
- **Slack**: Free tier sufficient for notifications

## Next Steps

1. **Customize AI prompts** for your specific business
2. **Add more routing rules** for your team structure
3. **Set up dashboard** for analytics
4. **Train your team** on how to respond to notifications
5. **Print QR codes** and distribute them in your location

## Support Resources

- [n8n Documentation](https://docs.n8n.io)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Google Sheets API Guide](https://developers.google.com/sheets/api)
- [Slack API Documentation](https://api.slack.com)

## Security Best Practices

- ✓ Keep API keys secure and never commit them to Git
- ✓ Use environment variables for sensitive data
- ✓ Regularly rotate API keys
- ✓ Limit OAuth scopes to minimum required
- ✓ Enable 2FA on all accounts
- ✓ Monitor API usage for unusual activity

---

**Need help?** Open an issue in this repository or check the [WORKFLOW.md](WORKFLOW.md) for detailed workflow documentation.
