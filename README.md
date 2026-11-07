# Repo Atlas

**Paste any public GitHub repo and watch it become a 3D city. Then let Claude, GPT and Gemini debate how good it is.**

Repo Atlas is a single static web page. There is no backend, no build step and nothing to install.

- **3D city map.** Folders become districts and files become buildings. A taller building means a bigger file. Hover for details, click to fly to a file, switch the colouring between file type, size and depth, isolate a language, or search by path.
- **Atlas score.** A quick 0-100 health check computed from the file tree and repo metadata: README, license, tests, CI, docs, contributing guide, recent activity and more. It is a heuristic, not a verdict on code quality.
- **AI Council.** Up to three models review the repo from different angles (Claude as the Architect, GPT as the Skeptic, Gemini as the Mentor). They cross-examine each other's opinions, then a chair issues a verdict, a score and first steps for a newcomer. One key is enough: that model plays all three roles (solo mode). With two or three keys, every seat uses its own model, which gives a real cross-model debate. Google's AI Studio offers a free Gemini key, so you can try the whole flow without paying.
- **Share card.** One click makes a 1200x630 PNG with an isometric render of the repo, its score and the council's headline, plus ready-to-post text.
- **Deep links.** Open `index.html?repo=owner/name` to load a repo directly.

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

To host it for free: **Settings → Pages → Deploy from a branch → `main` / root**.

## AI Council setup

Open the **AI Council** tab and paste a key for any provider you want to seat:

| Seat | Provider | Default model (editable) |
| --- | --- | --- |
| Claude, The Architect | Anthropic | `claude-sonnet-5-5` |
| GPT, The Skeptic | OpenAI | `gpt-6.1-sol` |
| Gemini, The Mentor | Google | `gemini-3.5-flash` |

Model names change often, so each one is an editable field. If a model name is rejected, the seat shows the provider's error and the rest of the council carries on.

### What happens to your keys and your data

- Keys live in the page's memory and disappear when you refresh. You can opt in to remembering them in `localStorage` from the Settings tab.
- Each key is sent only to its own provider, straight from your browser. There is no Repo Atlas server.
- The page's Content-Security-Policy only allows connections to `api.github.com` and the three AI providers.
- Repository text (file list, README excerpt, metadata) is sent to the AI providers when you convene the council. Do not point it at private code unless you are happy with that.
- Everything in the repo, README and file names is treated as untrusted data in the prompts, and model output is rendered as plain text, never as HTML.

## GitHub rate limits

Without a token, GitHub allows 60 API requests per hour per IP, and each repo costs three. A token with no scopes raises that to 5,000 per hour. Add one in **Settings**. Results are cached in `sessionStorage` for ten minutes.

## Limits you should know about

- Very large repos: GitHub truncates huge trees, and Repo Atlas draws at most the 12,000 largest files. A toast tells you when that happens.
- Building height is file size, not commit churn or complexity.
- The Atlas score looks only at the file tree and metadata. It cannot judge the code itself.
- Private repos need a GitHub token with access to them.
- The 3D view needs WebGL. Without it, the analysis, council and share card still work.

## Develop

The app is one file, `index.html`. The pure logic (tree model, treemap layout, health checks, prompt building, JSON parsing) sits between `// <pure>` and `// </pure>` markers so it can be tested without a browser:

```bash
node tests/pure.test.js
```

three.js `r128` and its `OrbitControls` are loaded from jsDelivr.

## Ideas

- Commit-activity heat, so recently changed files glow.
- Compare two repos side by side.
- Export the city as a GLB or an animated GIF.
- Bring your own model endpoint (Ollama, OpenRouter).

Issues and pull requests are welcome.

## License

MIT, see [LICENSE](LICENSE).
