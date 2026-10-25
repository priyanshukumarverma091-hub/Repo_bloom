# RepoBloom — Repository Intelligence

RepoBloom analyzes GitHub contributor records, recent commit metadata, actual changed-file patches, and issues. With an OpenRouter API key, it adds an LLM-generated overview and per-commit explanations.

## Run locally (Windows)

```bat
cd RepoBloom_Enhanced
py -m venv .venv
.venv\Scripts\activate
python -m pip install -r requirements.txt
copy .env.example .env
```

Edit `.env` and add:
- `GITHUB_TOKEN` (recommended; token needs repository read access for private repositories)
- `OPENROUTER_API_KEY` (optional; enables AI explanations)

Then run:

```bat
python app.py
```

Open http://127.0.0.1:5000

## What it does
- Retrieves contributor counts from GitHub.
- Inspects up to 12 recent commits and their changed files/diffs.
- Shows commit SHA, author identity reported by GitHub, date, additions/deletions, and filenames.
- Explains a selected commit using the actual GitHub diff when OpenRouter is configured.
- Summarizes returned issues as follow-up context.

## Important limitations
- GitHub's contributor endpoint may take time to update and can omit some contributors; it is not a perfect ownership ledger.
- A Git commit author is evidence of the recorded author identity, not proof of the person's job title or complete contribution.
- Diff patches can be absent for binary/large files and are truncated before sending to the LLM.
- The LLM may infer intent; its explanation should be checked against the linked source commit.
- Discord/Slack and documentation integrations are not included in this starter ZIP. They require separately configured integrations and permissions.
- This app is a local Flask app, not yet configured for Vercel/serverless deployment.

## Troubleshooting
- `GitHub authentication failed`: verify `GITHUB_TOKEN`.
- `Repository or resource not found`: check spelling, visibility, and token access.
- `rate limit`: add a token to `.env`.
- AI explanation unavailable: set `OPENROUTER_API_KEY`, restart Flask, and check the server log.
