# Contributing to EastClub Agent

Thanks for your interest in making EastClub Agent better! 🎉

This project is a **single-file Cloudflare Worker** — the whole app (frontend, backend, KV/D1/R2/DO logic, scheduling) lives in `worker.js`. Keep that constraint in mind: pull requests that introduce build steps, package managers or multiple runtime files are unlikely to be merged unless they bring a big benefit.

## Ways to contribute

### 🐛 Report a bug
- Open an issue using the **Bug report** template.
- Include: the Cloudflare bindings you configured (KV / D1 / R2 / DO), the exact URL/page, the steps to reproduce, and any console errors.
- Note whether you're on the deployed Worker or the local `eastclub-serve.mjs`.

### 💡 Suggest a feature
- Open an issue using the **Feature request** template.
- Explain the problem you're solving, not just the feature name.

### 🧪 Test & local dev
- Run the local mock server:
  ```bash
  node eastclub-serve.mjs   # serves on http://127.0.0.1:8787
  ```
- `EC_BINDINGS=0 node eastclub-serve.mjs` runs in pure-KV fallback mode.
- Before committing, always verify the worker parses:
  ```bash
  node --check worker.js
  ```

### 🔀 Submitting a pull request
1. Fork the repo and create a branch (`feat/...`, `fix/...`).
2. Keep the change minimal and self-contained.
3. If you touched `worker.js`, run `node --check worker.js` and confirm no syntax errors.
4. Fill in the pull-request template.
5. Keep it in one file when possible — this project is proudly single-file.

## Code of conduct

Be respectful. Harassment, trolling, and personal attacks are not welcome. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

By contributing, you agree that your contributions are licensed under the project's [MIT License](LICENSE).