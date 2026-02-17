# Daily Bean Feedback Agent

## 🎯 Introduction

The **Daily Bean Feedback Agent** is an intelligent automation system built with [n8n](https://n8n.io) that transforms how businesses collect and process customer feedback. By leveraging AI-powered automation, this agent eliminates manual data sorting and enables real-time intelligent routing of feedback.

### The Problem

Businesses often collect customer feedback through forms and surveys, but the feedback ends up in a single spreadsheet where:
- Everything is mixed together without categorization
- Manual sorting is time-consuming and error-prone
- Response times are delayed due to manual processing
- Insights are difficult to extract from unstructured data

### The Solution

The Daily Bean collects feedback through a simple QR-code accessible form, and everything goes into one Google Sheet. The n8n agent then:

1. **Monitors** the sheet for new feedback entries
2. **Analyzes** each entry using AI to understand sentiment, category, and priority
3. **Routes** feedback automatically to the appropriate team or action
4. **Provides** instant insights and structured data

## ✨ Benefits

- **⚡ Faster Reactions**: Automated routing means critical feedback reaches the right team immediately
- **📊 Clearer Insights**: AI-powered analysis categorizes and structures all feedback automatically
- **🚫 No Manual Sorting**: Save hours of manual work by letting AI handle categorization
- **📈 Scalable Solution**: Handle increasing feedback volume without additional manual effort
- **🔄 Real-time Processing**: New entries are processed as soon as they arrive

## 🏢 Business Impact

This automation solves real business problems:

- **Customer Service**: Route complaints immediately to support teams
- **Product Development**: Identify feature requests and bugs automatically
- **Management**: Get real-time dashboards of customer sentiment
- **Marketing**: Track campaign feedback and customer satisfaction

## 🔄 Reusability

The same structure can be adapted for various use cases across different industries:

- **Restaurant/Café Feedback** (The Daily Bean original use case)
- **Retail Customer Feedback**
- **Event Feedback Collection**
- **Product Reviews Management**
- **Employee Satisfaction Surveys**
- **Support Ticket Classification**
- **Lead Qualification**
- **Survey Response Processing**

## 🏗️ Technical Architecture

### Components

1. **Feedback Collection Layer**
   - QR code linking to Google Form
   - Responses stored in Google Sheets

2. **n8n Automation Agent**
   - Trigger: New row in Google Sheet
   - AI Analysis: OpenAI/Claude for sentiment and categorization
   - Router: Conditional logic based on AI analysis
   - Actions: Email notifications, Slack messages, database updates, etc.

3. **Output Destinations**
   - Team notification channels (Slack, Email, SMS)
   - CRM/Database updates
   - Analytics dashboards
   - Ticket systems

### Workflow Overview

```
QR Form → Google Sheet → n8n Trigger → AI Analysis → Smart Routing → Actions
```

## 🚀 Getting Started

### Prerequisites

- [n8n](https://n8n.io) instance (self-hosted or cloud)
- Google account (for Forms and Sheets)
- OpenAI API key or similar AI service
- Notification channels (Slack, Email, etc.)

### Installation

1. **Clone this repository**
   ```bash
   git clone https://github.com/amine1425/Daily-Bean-Feedback-Agent.git
   cd Daily-Bean-Feedback-Agent
   ```

2. **Import the n8n workflow**
   - Open your n8n instance
   - Go to Workflows → Import
   - Upload `daily-bean-feedback-workflow.json`

3. **Configure credentials**
   - Google Sheets API connection
   - OpenAI API key
   - Notification service credentials (Slack, Email, etc.)

4. **Customize routing rules**
   - Adjust AI prompts for your specific feedback categories
   - Configure routing logic based on your team structure
   - Set up notification preferences

### Setup

1. **Create your Google Form**
   - Add fields: Name, Email, Feedback Type, Message, Rating
   - Generate QR code linking to the form
   - Connect form responses to Google Sheet

2. **Configure the n8n workflow**
   - Point the Google Sheets trigger to your feedback sheet
   - Update AI prompts to match your business context
   - Set up routing destinations (Slack channels, email addresses, etc.)

3. **Test the workflow**
   - Submit a test feedback through the form
   - Verify the agent processes it correctly
   - Check that notifications are sent to the right channels

## 📖 Usage

### Basic Operation

1. **Share the QR code** with customers (printed on receipts, table tents, etc.)
2. Customers scan and **submit feedback** through the form
3. The agent **automatically processes** each entry
4. Teams receive **categorized notifications** in real-time
5. View **analytics and insights** in your dashboard

### AI Analysis

The agent analyzes each feedback entry for:
- **Sentiment**: Positive, Neutral, Negative
- **Category**: Complaint, Suggestion, Praise, Question, Bug Report
- **Priority**: High, Medium, Low
- **Keywords**: Extract main topics and themes

### Routing Examples

- **High-priority complaints** → Immediate alert to management + Support team
- **Feature requests** → Product team Slack channel
- **Positive reviews** → Marketing team + Request permission for testimonial
- **Questions** → Support ticket system
- **Low-priority feedback** → Weekly digest email

## 🛠️ Customization

You can easily adapt this agent for your specific needs:

1. **Modify AI prompts** to match your business domain
2. **Add custom categories** relevant to your industry
3. **Integrate with your tools** (CRM, help desk, analytics platforms)
4. **Adjust routing logic** to match your team structure
5. **Add additional processing steps** (translation, data enrichment, etc.)

## 📊 n8n Workflow

The complete n8n workflow is available in `daily-bean-feedback-workflow.json`. The workflow includes:

- Google Sheets trigger node
- AI analysis nodes (OpenAI/Claude)
- Conditional routing logic
- Multiple action nodes (Slack, Email, HTTP requests)
- Error handling and logging

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs or issues
- Suggest new features or improvements
- Share your customizations and use cases
- Submit pull requests

## 📝 License

This project is open source and available under the MIT License.

## 🙏 Acknowledgments

Built with [n8n](https://n8n.io) - the workflow automation platform
Powered by AI services (OpenAI, Anthropic Claude, etc.)

## 📧 Contact

For questions or support, please open an issue in this repository.

---

**⭐ If this agent helps your business, please star the repository!**