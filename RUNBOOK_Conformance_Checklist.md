# RUNBOOK Conformance Checklist (to validate against your repo)

Use this checklist to verify the RUNBOOK’s steps match actual scripts/logic:

## Environment & Secrets
- ✅ Environment variable names in RUNBOOK == used in code (`AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_DEPLOYMENT`, etc.)
- ✅ Key Vault secret names and paths match the scripts
- ✅ Model **names + versions** align with your allowed quota (e.g., `gpt-4o` `2024-08-06` vs. `2025-05-??`)
- ✅ Region in scripts matches your resource group region

## Script Entrypoints
- ✅ The scripts listed in RUNBOOK exist and are executable (e.g., `scripts/aoai_deploy_model.sh`, `scripts/spec_to_docs.py`)
- ✅ CLI flags and default paths match code (`--spec`, `--out`, `--model`)
- ✅ Any referenced Dockerfiles / workflow YAMLs actually exist

## Dependencies
- ✅ All Python requirements are pinned in `requirements.txt` (or `pyproject.toml`)
- ✅ Node packages declared in `package.json` (`@stoplight/spectral-cli`, `@redocly/cli`)
- ✅ Azure CLI extensions called in instructions are installed (`az containerapp`, `az apim`, etc.)

## Permissions
- ✅ Managed Identity / service principal has access to Key Vault secrets used at runtime
- ✅ Storage container permissions allow write for artifacts (docs, logs)

## LLM behaviour & Guardrails
- ✅ Temperature, max_tokens, and retry logic match RUNBOOK defaults
- ✅ Prompt templates referenced in RUNBOOK exist and are loaded by code
- ✅ Spectral ruleset path matches RUNBOOK and CI (same rules in both)

## Outputs
- ✅ Output paths in RUNBOOK equal what scripts actually write (e.g., `docs/api-docs.md`, `public/redoc.html`)
- ✅ CI publishes artifacts to expected location (e.g., Azure Storage/Build artifacts)
