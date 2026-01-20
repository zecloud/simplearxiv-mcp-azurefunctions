# ArXiv MCP Server on Azure Functions

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server running on Azure Flex Consumption Plan that provides ArXiv research paper search and retrieval capabilities.

## What is mcp-simple-arxiv?

[mcp-simple-arxiv](https://github.com/shaneholloman/mcp-simple-arxiv) is a Python MCP server that provides access to ArXiv research papers. It allows searching and retrieving academic papers from the ArXiv repository through a standardized MCP interface.

### Features

- **Search ArXiv papers**: Query the ArXiv database for research papers
- **Retrieve paper details**: Get metadata, abstracts, and information about papers
- **PDF access**: Access full-text PDFs of research papers
- **Category filtering**: Search by ArXiv categories and subjects

## Project Architecture

This project combines:
- **mcp-simple-arxiv**: For ArXiv paper search and retrieval
- **MCP (Model Context Protocol)**: To expose capabilities via a standardized protocol
- **Azure Functions (Flex Consumption Plan)**: For scalable and cost-effective serverless hosting

## Installation

### Prerequisites

- Python 3.10 or higher
- An Azure account (for deployment)
- Azure Functions Core Tools
- Azure CLI

### Local Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd arxivmcpfunctions
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**
   ```bash
   cp local.settings.json.example local.settings.json
   # Edit local.settings.json with your configurations
   ```

5. **Run locally**
   ```bash
   func start
   ```

## Deployment to Azure

1. **Login to Azure**
   ```bash
   az login
   ```

2. **Create Azure resources**
   ```bash
   # Create a resource group
   az group create --name arxiv-mcp-rg --location francecentral

   # Create a storage account
   az storage account create --name arxivmcpstorage --resource-group arxiv-mcp-rg --location francecentral

   # Create the Function App (Flex Consumption)
   az functionapp create --name arxiv-mcp-function \
     --resource-group arxiv-mcp-rg \
     --storage-account arxivmcpstorage \
     --runtime python \
     --runtime-version 3.10 \
     --functions-version 4 \
     --consumption-plan-location francecentral
   ```

3. **Deploy the code**
   ```bash
   func azure functionapp publish arxiv-mcp-function
   ```

## Usage

Once deployed, the MCP server exposes ArXiv search and retrieval capabilities via the MCP protocol. MCP clients can use these features to search and access research papers.

### Example usage with an MCP client

```python
# Example of searching ArXiv papers
result = mcp_client.call_tool(
    "search_arxiv",
    {
        "query": "quantum computing",
        "max_results": 10
    }
)
print(result)
```

## Flex Consumption Plan Benefits

- **Optimized costs**: Pay only for actual execution
- **Automatic scaling**: Automatically adapts to load
- **Fast startup**: Reduced cold start times
- **Azure integration**: Easy access to other Azure services

## License

This project is under MIT license.

## Resources

- [mcp-simple-arxiv GitHub](https://github.com/shaneholloman/mcp-simple-arxiv)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Azure Functions Documentation](https://docs.microsoft.com/azure/azure-functions/)
- [ArXiv.org](https://arxiv.org/)
