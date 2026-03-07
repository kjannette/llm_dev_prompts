---
title: Code Styleguide — JavaScript
last_modified: 2026-02-22
---

1.  Use `const` for all declarations, unless you need to reassign, then use `let`. Never use
   `var`.

2. Use npm for package management, avoid using yarn.

3. Use nvm and install/select the most current LTS Node version.

4. Use `prettier` for code formatting, with four spaces for indentation.

5.  At a minimum, both `npm run test`/`npm run build` should work (complete the appropriate scripts in `package.json`). However, prefer `make test` and `make build` instead —

6. The Makefile is authoritative on how to interact with the repo. See
   [Repository Policies](https://git.eeqj.de/sneak/prompts/raw/branch/main/prompts/REPO_POLICIES.md) for details.

# Author

[@sjDev](https://sjdev.co)
&lt;[sj@sjdev.co](mailto:sj@sjdev.co)&gt;

# License

MIT. See [LICENSE](../LICENSE).
