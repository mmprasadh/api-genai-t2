# Use Case 2 Quickstart — Generate User‑Friendly API Docs from an OpenAPI Spec (LLM‑assisted)

This guide lets you produce readable docs from an OpenAPI YAML (or JSON) using an LLM **without** requiring an MCP server. If your repo already includes scripts, replace steps 3–6 with your script names.

## 0) Prerequisites
- **OpenAPI spec** file: `specs/sample.yaml` (v3.0/3.1)
- **Azure OpenAI** (or OpenAI) endpoint + API key
- Local tools:
  - Python 3.10+
  - Node 18+ (for Spectral/Redocly)
  - `az` CLI (if fetching secrets from Key Vault)
  - `npm i -g @redocly/cli @stoplight/spectral-cli`

## 1) Validate & polish the spec
```bash
spectral lint specs/sample.yaml
# optional: redocly lint specs/sample.yaml
```

## 2) (Optional) Normalise the spec
```bash
npx @redocly/openapi-cli bundle specs/sample.yaml -o build/openapi.yaml --ext yaml
```

## 3) Generate first‑cut docs with the LLM (Python example)
Save as `scripts/spec_to_docs.py`:
```python
import os, yaml, textwrap
from openai import OpenAI

spec_path = os.environ.get("OPENAPI_SPEC", "build/openapi.yaml")
model = os.environ.get("DOCS_MODEL", "gpt-4o-mini")
client = OpenAI(base_url=os.environ.get("OPENAI_BASE_URL"), api_key=os.environ["OPENAI_API_KEY"])

prompt = lambda spec: f"""You are a senior API technical writer.
Turn this OpenAPI spec into friendly docs for developers:
- Overview (what/why)
- Auth (keys/tokens)
- How to start (curl + 200 example)
- Endpoints: method, path, params, request/response examples
- Error handling table
- Rate limits, pagination
- Best practices
Write clean Markdown with headers and code fences.
SPEC:
{spec}
"""

with open(spec_path, "r") as f:
    spec_text = f.read()

resp = client.chat.completions.create(
    model=model,
    messages=[{"role":"user","content":prompt(spec_text)}],
    temperature=0.2,
)
md = resp.choices[0].message.content
os.makedirs("docs", exist_ok=True)
out = "docs/api-docs.md"
with open(out, "w") as f:
    f.write(md)
print(f"Wrote {out}")
```

Run it:
```bash
export OPENAI_API_KEY=***        # or AZURE_OPENAI_KEY etc.
export OPENAI_BASE_URL=***       # e.g. https://{your-aoai}.openai.azure.com/v1
export DOCS_MODEL=gpt-4o-mini    # any model approved in your tenancy
export OPENAPI_SPEC=build/openapi.yaml

python scripts/spec_to_docs.py
```

## 4) Render as interactive HTML (Redoc)
```bash
redocly build-docs build/openapi.yaml -o public/redoc.html
# Optionally merge with LLM md
cat docs/api-docs.md > public/README.md
```

## 5) (Optional) Produce a printable PDF
Use any Markdown→PDF tool in your environment, e.g.
```bash
npx md-to-pdf docs/api-docs.md
```

## 6) Automate in CI
- Step 1: Lint spec with Spectral
- Step 2: Run `spec_to_docs.py`
- Step 3: Publish artifacts (Markdown, HTML, PDF) to build outputs or a storage container
- Step 4: Optional — push `public/redoc.html` to APIM developer portal

---

### Do I need an MCP server for Use Case 2?
**No**, not inherently. For “spec → docs”, calling the LLM directly is simpler and faster.  
Use an MCP server only if you need a **standardised tool interface** for the model (e.g., reading specs from GitHub/Jira, writing to Storage, or querying org datasets).

If you do want MCP, prefer **HTTP+SSE** for Azure-hosted, multi-user scenarios (see the MCP section in the other doc).
