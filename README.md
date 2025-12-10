# 🤖 N8N AI News Summarizer

An intelligent N8N workflow that leverages AI agents to fetch, analyze, rank, and deliver personalized news digests directly to your inbox. Features an interactive chatbot for conversational news Q&A.

[![N8N](https://img.shields.io/badge/N8N-Workflow-EA4B71?style=flat&logo=n8n)](https://n8n.io)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## 🌟 Features

### Core Capabilities
- **📰 Automated News Fetching**: Pulls latest headlines from NewsAPI across multiple categories
- **🧠 AI-Powered Summarization**: Generates concise 5-bullet summaries using Groq LLM (Qwen3-32B)
- **🎯 Intelligent Ranking**: Ranks articles by importance based on recency and source authority
- **📧 Beautiful Email Digests**: Sends professionally formatted HTML newsletters
- **💬 Interactive Chatbot**: Ask questions about the news via conversational interface
- **🧩 AI Agent Architecture**: Uses LLM + Memory + Tools pattern for intelligent automation

### Workflow Components
1. **Chat Trigger**: Receives user messages
2. **AI Agent**: Central intelligence hub with:
   - **LLM**: Groq Chat Model (Qwen3-32B) for reasoning & content generation
   - **Memory**: Buffer window for conversation context
   - **Tools**: HTTP Request (news fetching) & Gmail (email delivery)

## 📋 Prerequisites

- [N8N](https://n8n.io) installed (self-hosted or cloud)
- [NewsAPI](https://newsapi.org) account & API key
- Gmail account with OAuth2 configured
- Groq API account for LLM access

## 🚀 Installation

### 1. Clone Repository
```bash
git clone https://github.com/Rahul4112002/n8n-ai-news-summarizer.git
cd n8n-ai-news-summarizer
```

### 2. Configure Environment Variables
Create a `.env` file (or configure in N8N):
```bash
cp .env.example .env
```

Edit `.env` with your credentials:
```env
# NewsAPI Configuration
NEWS_API_KEY=your_newsapi_key_here

# Groq API Configuration  
GROQ_API_KEY=your_groq_api_key_here

# Gmail OAuth2 (configure in N8N UI)
# Follow N8N Gmail OAuth2 setup guide
```

### 3. Import Workflow to N8N

**Option A: Via N8N UI**
1. Open your N8N instance
2. Click **Workflows** → **Import from File**
3. Select `workflow.json`
4. Click **Import**

**Option B: Via URL**
1. Go to **Workflows** → **Import from URL**
2. Paste: `https://raw.githubusercontent.com/Rahul4112002/n8n-ai-news-summarizer/main/workflow.json`

### 4. Configure Credentials

Set up the following credentials in N8N:

#### a) NewsAPI (HTTP Header Auth)
- **Name**: `Header Auth account`
- **Header Name**: `X-Api-Key`
- **Header Value**: `<your-newsapi-key>`

#### b) Groq API
- **Name**: `Groq account`
- **API Key**: `<your-groq-api-key>`

#### c) Gmail OAuth2
- **Name**: `Gmail account`
- Follow [N8N Gmail OAuth2 setup guide](https://docs.n8n.io/integrations/builtin/credentials/google/#oauth2)

### 5. Activate Workflow
1. Open the imported workflow
2. Click **Active** toggle in top-right
3. Workflow is now ready to use!

## 💡 Usage

### Chatbot Mode
Send messages to the workflow chat trigger:

```
User: "What's the latest in AI news?"
Bot: [Provides summary of recent AI articles]

User: "Send me a digest of tech and business news to rahul@example.com"
Bot: [Fetches news, generates HTML digest, sends email]
```

### Email Digest Request Format
To trigger email delivery, include:
1. **Email address** (e.g., `yourname@example.com`)
2. **Clear request** (e.g., "send digest", "email me news")

**Example:**
```
Send today's AI and technology news to john.doe@gmail.com
```

### Supported News Categories
- `business`
- `entertainment`
- `general`
- `health`
- `science`
- `sports`
- `technology`

## 🏗️ Architecture

### AI Agent Components

```
┌─────────────────────────────────────────┐
│         Chat Trigger (Webhook)          │
└──────────────────┬──────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│            AI Agent (Core)              │
│  ┌──────────────────────────────────┐   │
│  │  LLM: Groq (Qwen3-32B)           │   │
│  │  - Planning & Reasoning          │   │
│  │  - Content Generation            │   │
│  └──────────────────────────────────┘   │
│  ┌──────────────────────────────────┐   │
│  │  Memory: Buffer Window           │   │
│  │  - Conversation Context          │   │
│  └──────────────────────────────────┘   │
│  ┌──────────────────────────────────┐   │
│  │  Tools:                          │   │
│  │  1. HTTP Request (NewsAPI)       │   │
│  │  2. Gmail (Email Delivery)       │   │
│  └──────────────────────────────────┘   │
└─────────────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│         Chat Response / Email           │
└─────────────────────────────────────────┘
```

### Email Template Design
- **Responsive HTML** with inline CSS for maximum compatibility
- **Gradient Header** with purple branding
- **Featured Section** for top 5 stories with large images
- **Category Sections** with thumbnails and 5-bullet summaries
- **Modern Typography** hierarchy for readability

## 🎨 Customization

### Modify AI System Prompt
Edit the `systemMessage` in the **AI Agent** node to change:
- Summary style/length
- Ranking criteria
- Email tone
- Default categories

### Adjust News Sources
In the **HTTP Request** node, modify:
- `pageSize`: Number of articles per category (default: 28)
- `sortBy`: `relevance` or `popularity`
- Query parameters for filtering

### Email Template
The HTML template is embedded in the system prompt. Key sections:
- Header gradient: `#667eea` to `#764ba2`
- Featured articles: 180x120px images
- Regular articles: 100x75px thumbnails

## 🛠️ Troubleshooting

### No News Results
- Verify NewsAPI key is valid and has remaining quota
- Check category spelling (must be lowercase)
- Ensure search query is relevant

### Email Not Sending
- Confirm Gmail OAuth2 is properly configured
- Check recipient email format
- Verify `sendEmail` flag is triggered in conversation

### AI Agent Not Responding
- Check Groq API key and quota limits
- Review system prompt for syntax errors
- Ensure memory node is connected

## 📊 Workflow Goals

As outlined in the workflow:
1. ✅ Fetch latest news from the web → **API Tool**
2. ✅ Summarize news in 5 bullet points → **Brain (LLM)**
3. ✅ Chatbot for Q&A about news → **Brain (LLM + Memory)**
4. ✅ Rank news by importance → **Brain (LLM)**
5. ✅ Send formatted digest via email → **Brain + Gmail Tool**

## 🔐 Security Notes

- **Never commit** `.env` or credentials to version control
- Use **environment variables** for sensitive data
- Enable **OAuth2** for Gmail (avoid app passwords)
- Rotate API keys regularly
- Review N8N [security best practices](https://docs.n8n.io/hosting/security/)

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📧 Contact

**Rahul** - [@Rahul4112002](https://github.com/Rahul4112002)

**Project Link**: [https://github.com/Rahul4112002/n8n-ai-news-summarizer](https://github.com/Rahul4112002/n8n-ai-news-summarizer)

## 🙏 Acknowledgments

- [N8N](https://n8n.io) - Workflow automation platform
- [NewsAPI](https://newsapi.org) - News aggregation API
- [Groq](https://groq.com) - Fast AI inference
- [Qwen](https://qwenlm.github.io/) - Open-source LLM

---

Made with ❤️ using N8N AI Agents