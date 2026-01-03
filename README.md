# Release Notes Agent - DevOps to Confluence

_Powered by Google ADK, Azure DevOps MCP & Atlassian MCP_

**This agent automates release notes creation from Azure DevOps user stories and publishes them to Confluence.**

### How It Works

1. **Fetch Tickets** - Retrieves work items from a configured Azure DevOps query
2. **Extract Data** - Gets ticket details: ID, Title, Description, Release Date, Story Owner, Area, Customer, URL
3. **Generate Release Notes** - Creates user-friendly summaries from technical descriptions
4. **Save to Excel** - Stores all data incrementally in an Excel file (prevents data loss)
5. **Publish to Confluence** - Creates a formatted Markdown page under the specified parent page

### Architecture
```
┌─────────────────────────────────────────────────────────────┐
│                    SequentialAgent                          │
│                  (ReleaseNotesWorkflow)                     │
├─────────────────────────┬───────────────────────────────────┤
│  TicketProcessorAgent   │     ConfluencePublisherAgent      │
│  ┌───────────────────┐  │     ┌───────────────────────┐     │
│  │ Azure DevOps MCP  │  │     │ read_excel_data       │     │
│  │ initialize_excel  │  │────▶│ format_confluence     │     │
│  │ append_ticket     │  │     │ Atlassian MCP         │     │
│  └───────────────────┘  │     └───────────────────────┘     │
└─────────────────────────┴───────────────────────────────────┘
```

### Prerequisites

- **Python 3.10+** with virtual environment
- **Node.js 18+** - Required for MCP servers (npx)
- **Azure DevOps Access** - Browser authentication triggered on first run
- **Atlassian Cloud Access** - Browser authentication triggered on first run
- **Environment Variables** - `GOOGLE_API_KEY` in `.env` file

### Configuration

Update these variables in the notebook to match your setup:

| Variable | Description |
|----------|-------------|
| `AZURE_DEVOPS_ORG` | Your Azure DevOps organization name |
| `AZURE_DEVOPS_PROJECT` | Project containing work items |
| `AZURE_DEVOPS_QUERY_ID` | Saved query ID returning tickets for release notes |
| `MODEL_NAME` | Gemini model to use |
| `spaceId` | Confluence space ID (in agent instruction) |
| `parentPageId` | Parent page ID for release notes (in agent instruction) |

### Usage
```python
await chat()
# Then type: "Create release notes and publish to Confluence"
```
