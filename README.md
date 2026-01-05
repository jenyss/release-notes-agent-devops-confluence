# Release Notes Agent - DevOps to Confluence

_Powered by Google ADK, Azure DevOps MCP & Atlassian MCP_

**This agent automates release notes creation from Azure DevOps user stories and publishes them to Confluence.**

**Coming soon...**
[![Watch on YouTube](https://img.shields.io/badge/Watch%20on-YouTube-red?logo=youtube&style=for-the-badge)](https://youtu.be/dkxzehq6E8o)

If you have any questions or would like to collaborate, feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/jenya-stoeva-60477249/). You're more than welcome!

### How It Works

1. **Fetch Tickets** - Retrieves work items from a configured Azure DevOps query
2. **Extract Data** - Gets ticket details: ID, Title, Description, Release Date, Story Owner, Area, Customer, URL
3. **Generate Release Notes** - Creates user-friendly summaries from technical descriptions
4. **Save to Excel** - Stores all data incrementally in an Excel file (prevents data loss)
5. **Publish to Confluence** - Creates a formatted Markdown page under the specified parent page

### Architecture

**TicketProcessorAgent**: Connects to the Azure DevOps MCP server, fetches work items, generates user‑friendly release notes, and writes all processed ticket data into a structured Excel file.

**ConfluencePublisherAgent**: Loads the generated Excel data, formats it into Confluence‑ready Markdown, connects to the Atlassian MCP server, and publishes the final release notes page into the correct Confluence space.

These two agents are orchestrated by SequentialAgent, which runs the workflow sequentially: first generating release notes, then publishing them.

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

### How-To
**TODO**

How to get Confluence space and page IDs

```
Use: https://<confluence-site>.atlassian.net/wiki/api/v2/spaces?keys=<space-key>
Example: https://marmind-knowledgebase.atlassian.net/wiki/api/v2/spaces?keys=KB
```

```
{
  "results": [
    {
      "spaceOwnerId": "712020:e59583ce-74ea-4967-9919-6d15e6ab4f51",
      "homepageId": "327875",
      "createdAt": "2025-06-25T07:27:18.843Z",
      "authorId": "712020:e59583ce-74ea-4967-9919-6d15e6ab4f51",
      "icon": null,
      "description": null,
      "status": "current",
      "name": "Marmind Knowledge Base",
      "key": "KB",
      "id": "327684",
      "type": "knowledge_base",
      "_links": {
        "webui": "/spaces/KB"
      },
      "currentActiveAlias": "KB"
    }
  ],
  "_links": {
    "base": "https://marmind-knowledgebase.atlassian.net/wiki"
  }
}
```

```python
await chat()
# Then type: "Create release notes and publish to Confluence"
```
