# Implementation notes

## Agentcraft Field Guide v0 scaffold

- Fast-tracked `proposals/agentcraft-field-guide.md` as the canonical active plan.
- Keep v0 in this repo and use root-level Jekyll/GitHub Pages source to avoid duplicating Markdown.
- Use custom minimal Jekyll layout/CSS instead of adding a theme dependency or Gemfile.
- Exclude `ideas/`, `proposals/`, and `experiments/` from the published site by default so the lab notebook remains visible in the repo but not in the main Field Guide site.
- Add initial `cheatsheets/` and `guides/` sections with one published page each to satisfy the smallest useful Field Guide shape.
- Set `baseurl: /agentcraft` for the default GitHub Pages project-site URL.
- Verified the site builds with `jekyll build --destination /tmp/agentcraft-field-guide-site` and excludes rough notebook directories from the generated site.
