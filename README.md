# Azure-RAG-Chat-Python - A RAG Chat with Azure-AI Search OpenAI & Python
A sample application demonstrating Retrieval Augmented Generation (RAG) with Azure AI Search and Azure OpenAI Service. This project creates a ChatGPT-like experience over your own documents, combining powerful GPT models with enterprise-grade search for knowledge retrieval.

## Features:
- Multi-turn chat interface with citations and reasoning steps
- Document indexing and retrieval via Azure AI Search (supports PDFs, docs, cloud data)
- Configurable UI for experimenting with prompts and behaviors
- Optional multimodal support for image-heavy documents
- Speech input/output for accessibility
- Microsoft Entra integration for secure login and data access
- Performance monitoring with Application Insights

![Chat screen](chatscreen.png)

## Architecture Diagram
![Architecture Diagram](appcomponents.png)

## Azure account requirements

**IMPORTANT:** In order to deploy and run this example, you'll need:

- **Azure account**. If you're new to Azure, [get an Azure account for free](https://azure.microsoft.com/free/cognitive-search/) and you'll get some free Azure credits to get started. See [guide to deploying with the free trial](docs/deploy_freetrial.md).
- **Azure account permissions**:
  - Your Azure account must have `Microsoft.Authorization/roleAssignments/write` permissions, such as [Role Based Access Control Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview), [User Access Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator), or [Owner](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner). If you don't have subscription-level permissions, you must be granted [RBAC](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview) for an existing resource group and [deploy to that existing group](docs/deploy_existing.md#resource-group).
  - Your Azure account also needs `Microsoft.Resources/deployments/write` permissions on the subscription level.

### Cost estimation

Pricing varies per region and usage, so it isn't possible to predict exact costs for your usage.
However, you can try the [Azure pricing calculator](https://azure.com/e/e3490de2372a4f9b909b0d032560e41b) for the resources below.

- Azure Container Apps: Default host for app deployment as of 10/28/2024. See more details in [the ACA deployment guide](docs/azure_container_apps.md). Consumption plan with 1 CPU core, 2 GB RAM, minimum of 0 replicas. Pricing with Pay-as-You-Go. [Pricing](https://azure.microsoft.com/pricing/details/container-apps/)
- Azure Container Registry: Basic tier. [Pricing](https://azure.microsoft.com/pricing/details/container-registry/)
- Azure App Service: Only provisioned if you deploy to Azure App Service following [the App Service deployment guide](docs/azure_app_service.md).  Basic Tier with 1 CPU core, 1.75 GB RAM. Pricing per hour. [Pricing](https://azure.microsoft.com/pricing/details/app-service/linux/)
- Azure OpenAI: Standard tier, GPT and Ada models. Pricing per 1K tokens used, and at least 1K tokens are used per question. [Pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)
- Azure AI Document Intelligence: SO (Standard) tier using pre-built layout. Pricing per document page, sample documents have 261 pages total. [Pricing](https://azure.microsoft.com/pricing/details/form-recognizer/)
- Azure AI Search: Basic tier, 1 replica, free level of semantic search. Pricing per hour. [Pricing](https://azure.microsoft.com/pricing/details/search/)
- Azure Blob Storage: Standard tier with ZRS (Zone-redundant storage). Pricing per storage and read operations. [Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/)
- Azure Cosmos DB: Only provisioned if you enabled [chat history with Cosmos DB](docs/deploy_features.md#enabling-persistent-chat-history-with-azure-cosmos-db). Serverless tier. Pricing per request unit and storage. [Pricing](https://azure.microsoft.com/pricing/details/cosmos-db/)
- Azure AI Vision: Only provisioned if you enabled [multimodal approach](docs/multimodal.md). Pricing per 1K transactions. [Pricing](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/)
- Azure AI Content Understanding: Only provisioned if you enabled [media description](docs/deploy_features.md#enabling-media-description-with-azure-content-understanding). Pricing per 1K images. [Pricing](https://azure.microsoft.com/pricing/details/content-understanding/)
- Azure Monitor: Pay-as-you-go tier. Costs based on data ingested. [Pricing](https://azure.microsoft.com/pricing/details/monitor/)



## 🚀 Quickstart

This project demonstrates a **Retrieval Augmented Generation (RAG)** chat experience using **Azure AI Search** and **Azure OpenAI Service** with Python. You can run it in three ways: GitHub Codespaces, VS Code Dev Containers, or your local environment.

### 1. Run in GitHub Codespaces (Cloud-based)
The fastest way to try this repo without installing anything locally.

1. Click **Open in GitHub Codespaces** from the repo.
2. Wait for the codespace to initialize (may take several minutes).
3. Once VS Code opens in your browser, open a terminal window.

### 2. Run in VS Code Dev Containers (Local with Docker)
A reproducible local environment using Docker.

1. Install **Docker Desktop** (start it after installation).
2. Install the **Dev Containers extension** in VS Code.
3. Open the project in VS Code → select **Open in Dev Container**.
4. Wait for the container to build (may take several minutes).
5. Once the project files appear, open a terminal window.


### 3. Run in Local Environment
For full control and direct deployment to Azure.

#### Prerequisites
- [Azure Developer CLI](https://learn.microsoft.com/azure/developer/azure-developer-cli/overview)
- Python 3.10–3.14 (ensure `python --version` works)
- Node.js 20+
- Git
- PowerShell 7+ (Windows only, ensure `pwsh.exe` runs)

#### Setup
```bash
# Create a new folder and switch to it
mkdir my-rag-chat && cd my-rag-chat

# Initialize the project
azd init -t azure-search-openai-demo
```

### 4. Deploying to Azure
This provisions resources and deploys the app.
```bash
# Login to Azure
azd auth login
# (Codespaces users may need: azd auth login --use-device-code)

# Create a new environment
azd env new

# Deploy resources and app
azd up
```
  - **⚠️ Note:** Resources incur costs immediately (especially AI Search). Run `azd down` to delete resources when finished.
  - After deployment, a URL will be printed. Open it in your browser to use the app.
Deployment may take 5–10 minutes after `SUCCESS`.

### 5. Redeploying
  - If only app code changed:
    ```bash
    azd deploy
    ```
  - If infra files changed (`infra`/ or `azure.yaml`):
    ```bash
    azd up
    ```

### 6. Running Locally
You can run the dev server after a successful azd up.
  ```bash
  # Login if needed
  azd auth login
  
  # Start the server
  # Windows
  ./app/start.ps1
  
  # Linux/Mac
  ./app/start.sh
  ```
  Or run the **VS Code Task: Start App.**
  Access locally at: `http://127.0.0.1:50505`

### 7. Using the App
  - Ask questions about the sample company data (benefits, policies, roles).
  - Try multi-turn conversations, clarifications, and follow-ups.
  - Explore citations and sources.
  - Use **Settings** to tweak prompts and behaviors.
    
### 8. Clean Up
  To delete all Azure resources created:
  Confirm with `y` when prompted. This deletes the resource group and all resources.
```bash
azd down
```
