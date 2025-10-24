<h1 style="text-align: center;">Azure DevOps Comments Monitor</h1>
<p style="text-align: center;"><em>Powered by Google ADK &amp; Azure DevOps MCP</em></p>

**This agent automatically monitors Azure DevOps work items for mentions of specific team members and delivers daily email summaries.** 

The agent works with any Azure DevOps organization and is perfect for engineering managers, team leads, and individual contributors who need to stay informed about discussions, decisions, and action items where they're mentioned.

### How It Works

1. **Query Work Items** - Retrieves work items from a saved Azure DevOps query
2. **Fetch Comments** - Collects all comments from each work item
3. **Filter & Process** - Identifies mentions of the target user within the last 2 days
4. **Format Report** - Creates a structured summary with work item context and full comment text
5. **Send Notification** - Delivers the formatted report via email


### Prerequisites

To run the Azure DevOps Mentions Monitor agent with an MCP connection, you need:

* **Python 3.10+** with a virtual environment (venv)
* **JupyterLab** (installed inside the venv) or Google Colab
* **Node.js** - includes npx, which is used to launch the Azure DevOps MCP server (@azure-devops/mcp) on the fly without a global install
* **Azure DevOps Access** - Valid Personal Access Token (PAT) with Work Items (Read) permissions
* **Email Credentials** - Gmail account with App Password for sending notifications
* **Environment Variables** configured in `.env` file:
  * `GOOGLE_API_KEY` - Gemini API key for the agent's LLM
  * `EMAIL_USER` - Gmail address for sending notifications
  * `EMAIL_PASS` - Gmail App Password
  * `TO_EMAIL` - Recipient email address for summaries


### Customization

You can easily customize the agent by modifying:

* **Target User** - Change `@Jenya Stoeva` to any user's display name
* **Time Window** - Adjust the 2-day filter to any desired timeframe
* **Query ID** - Point to different saved queries in your Azure DevOps organization
* **Email Format** - Modify the email template and subject line
* **Organization/Project** - Configure for any Azure DevOps organization and project

```
┌───────────────────────────────────────────────────────────────┐
│                    USER TRIGGERS AGENT                        │
│                 (Types command in Jupyter)                    │
└────────────────────────┬──────────────────────────────────────┘
                         │
                         ▼
┌───────────────────────────────────────────────────────────────┐
│                   AGENT ORCHESTRATION                         │
│              (Google ADK + Gemini 2.5 Flash)                  │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  Session Manager                                        │  │
│  │  • Maintains conversation state                         │  │
│  │  • Tracks current_date for filtering                    │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  Instruction Engine (Workflow Controller)               │  │
│  │  • Step 1: Get work items from query                    │  │
│  │  • Step 2: Retrieve comments for each work item         │  │
│  │  • Step 3: Filter by date & mentions                    │  │
│  │  • Step 4: Send email notification                      │  │
│  └─────────────────────────────────────────────────────────┘  │
└────────────┬───────────────────────────┬──────────────────────┘
             │                           │
             ▼                           ▼
    ┌─────────────────────────┐    ┌──────────────────────────┐
    |  Azure DevOps MCP Tool  │    │  Email Notification Tool │
    │  (via npx)              │    │  (Gmail SMTP)            │
    └─────────────────────────┘    └──────────────────────────┘
```
### How-To

## How-To

**Prerequisites** to run the **Azure DevOps Comments Monitor agent** with an MCP connection, you need:

* Python 3.10+ with a virtual environment (venv)
* JupyterLab (installed inside the venv) to run this notebook
* Node.js - includes npx, which is used to launch the Azure DevOps MCP server (@azure-devops/mcp) on the fly without a global install
* Azure DevOps Personal Access Token (PAT) with Work Items (Read) permissions for your organization
* Gmail account with App Password enabled for sending email notifications
* Environment variables configured in a `.env` file:
  * `GOOGLE_API_KEY` - Your Gemini API key
  * `AZURE_DEVOPS_PAT` - Your Azure DevOps Personal Access Token
  * `EMAIL_USER` - Your Gmail address
  * `EMAIL_PASS` - Your Gmail App Password
  * `TO_EMAIL` - Recipient email address for summaries
