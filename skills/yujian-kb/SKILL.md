---
name: yujian-kb
description: Use when the user asks for Yujian company knowledge, store SOP, internal business rules, approved Wiki knowledge, or wants Codex to answer from the Yujian enterprise knowledge base. Uses the bundled yujian-kb light client and answers only from its response.
---

# yujian-kb

Use this Skill to access approved Yujian enterprise knowledge through the remote Knowledge QA Service at `https://yujian-kb.xaidev.tech`.

Business users install this Skill, not a separate npm CLI. The bundled script is the light Knowledge Access Client:

```bash
${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb/scripts/yujian-kb
```

Inside this repository, use:

```bash
skills/yujian-kb/scripts/yujian-kb
```

The script stores only a server URL and Knowledge Access Token, calls `/auth/status` and `/ask`, and prints the service response. It does not store knowledge locally, run gbrain, build indexes, or generate business answers.

## Core Rules

- Use `ask` only. Do not use `search`, MCP, local indexes, local embeddings, or gbrain directly.
- Use `https://yujian-kb.xaidev.tech` as the default server. Do not use `127.0.0.1:8000` or `5173`.
- Use the bundled script; do not require the user to install `@lefit/yujian-kb-cli`.
- Do not call service APIs by hand. The bundled script is the only access path this Skill should use.
- Base the answer on the script response. Preserve the CLI response structure by default, especially sections such as key actions, suggested wording, citations, and cautions.
- Do not summarize away operational structure unless the user explicitly asks for a brief summary.
- Do not add business rules, conclusions, promises, or operational steps that are not supported by the script response.
- If the script returns no trusted answer, tell the user the knowledge base did not find a trusted answer.
- Never write tokens into docs, issues, logs, or final answers. Do not repeat a token back to the user.

## Workflow

1. Resolve the bundled client path:

```bash
YUJIAN_KB="${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb/scripts/yujian-kb"
```

If working inside this repository, use:

```bash
YUJIAN_KB="skills/yujian-kb/scripts/yujian-kb"
```

2. Check authentication:

```bash
"$YUJIAN_KB" auth status --verify
```

3. If authentication is missing or invalid, ask the user for the Knowledge Access Token issued by an administrator. Use it only once:

```bash
"$YUJIAN_KB" auth login --token <token>
```

The default server is `https://yujian-kb.xaidev.tech`. Only override it when the user explicitly gives another server:

```bash
"$YUJIAN_KB" auth login --server <server-url> --token <token>
```

After initialization, do not mention or display the token.

4. Ask the question:

```bash
"$YUJIAN_KB" ask "<question>"
```

5. Answer the user from the script response. Keep structured sections intact when the response is structured. Preserve any no-answer result instead of filling gaps from general model knowledge.

## Boundaries

This Skill is the business-facing access package. The bundled script is a light client for the remote Knowledge QA Service; it is not the knowledge base backend. It does not upload files, review Wiki pages, publish knowledge, manage permissions, choose workspaces, choose sources, run gbrain, or generate its own business answer. Those responsibilities belong to the remote Knowledge QA Service and the knowledge operations backend.
