# RepoBloom n8n workflow

This folder contains an importable starter workflow for the existing RepoBloom project.

## What it does

1. Receives a POST request on the `repobloom-analysis` webhook.
2. Builds an evidence-first handoff prompt from the supplied GitHub and Discord context.
3. Calls OpenRouter for analysis.
4. Returns structured JSON to the caller.

**Important:** this workflow does not fetch GitHub or Discord data itself yet. The existing Flask backend should collect those sources and send them as `github_data` and `discord_data`. That is the planned next integration step in `app.py`.

## Import

1. Open your n8n instance.
2. Choose **Workflows → Import from File** and select `workflow.json`.
3. Open **Analyze with OpenRouter** and replace `REPLACE_WITH_OPENROUTER_API_KEY` with your key (or switch the node to an n8n-managed credential before sharing/deploying).
4. Select a model available to your OpenRouter account if `qwen/qwen3.7-plus` is unavailable.
5. Save the workflow and click **Test workflow**. Use the Test URL first; production URL works after the workflow is activated.

Do not commit real API keys or tokens to GitHub.

## Webhook request

Use `examples/request.json` as the body. The webhook expects JSON and requires `repo_url`. `github_data` and `discord_data` are optional context objects.

## Endpoint

After importing, the production webhook path is:

`/webhook/repobloom-analysis`

The complete URL depends on your n8n host, for example `https://YOUR-N8N-HOST/webhook/repobloom-analysis`.

## Current scope

This is the n8n folder/workflow starter only. It does not modify `app.py` or the existing UI. Next step is to connect the Flask backend to this webhook and map the returned `analysis` into RepoBloom's current result format.
