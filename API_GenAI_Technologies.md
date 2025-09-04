# Technologies & Components for GenAI + APIM Use Cases

This list captures the typical stack used to implement the two use cases:
1) **Generate API Spec using an LLM**, and  
2) **Generate API Documentation from an API Spec using an LLM**.

> Note: This is a generalised inventory based on best practices. I’ll refine it to your repo once I can open the code or a link to it.

## Azure Services
- **Azure OpenAI Service** (models such as GPT‑4o / o3 / text-embedding‑3-*; region + model version matter)
- **Azure API Management (APIM)** (optional runtime surface + developer portal)
- **Azure Key Vault** (secrets for API keys, connection strings)
- **Azure Container Apps _or_ App Service** (to host an MCP server or web/API workers)
- **Azure Storage (Blob)** (persist prompts, specs, outputs, Markdown/HTML/PDF)
- **Application Insights + Log Analytics** (telemetry, prompt/response logging if compliant)
- **Azure DevOps Pipelines _or_ GitHub Actions** (CI/CD for linting/spec generation/docs)
- **Azure Container Registry (ACR)** (container images for MCP servers or workers)
- **Managed Identity** (secure access to Key Vault / Storage without secrets)
- **(Optional) Azure Functions** (serverless jobs for batch/spec-to-doc transforms)
- **(Optional) Azure AI Foundry / Prompt Flow** (prompt orchestration & evaluations)

## Languages & Runtimes
- **Python 3.10+** (scripts, CLI tools, doc generation)
- **Node.js 18+ / TypeScript** (Spectral, Redocly CLI, dev tooling)
- **Bash / PowerShell** (setup & deployment)
- **YAML** (OpenAPI specs, CI pipelines, Spectral rulesets)

## Key SDKs & Libraries
- **OpenAI / Azure OpenAI SDKs** (`openai` Python, `openai` Node)
- **Spectral** (OpenAPI linting & quality gates)
- **Redocly CLI / Redoc** (interactive docs from OpenAPI)
- **openapi-spec-validator / prance / jsonschema** (schema validation)
- **PyYAML / ruamel.yaml** (YAML processing)
- **Jinja2 / Markdown / md-to-pdf** (templated, printable docs)
- **FastAPI/Flask or Express** (if hosting a helper API or SSE MCP server)
- **Requests / httpx / axios** (HTTP calls)

## Command-line Tools
- **Azure CLI** (`az`) and **Bicep/Terraform** (if IaC used)
- **Node/npm** (install Spectral/Redocly)
- **jq / yq** (JSON/YAML manipulation)
- **Git**

## Testing & Quality
- **Spectral ruleset** (extends `spectral:oas` + org style guide)
- **Pydantic/pytest** (if Python services)
- **ESLint/Prettier** (if TypeScript tools)

## Artifacts Produced
- **OpenAPI v3.1 YAML/JSON** (generated or refined spec)
- **Developer-friendly Docs** (Markdown + HTML via Redoc or md-to-pdf)
- **Lint Reports** (SARIF/JSON for CI annotations)
- **Prompt Logs & Evals** (if using Prompt Flow/evals)
