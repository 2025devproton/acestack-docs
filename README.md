# AceStack Docs

[![Docs](https://img.shields.io/badge/docs-documentation-blue?style=flat-square&logo=readthedocs&logoColor=white)](https://acestack-org.github.io/acestack-docs/)
<br>
[![GitHub stars](https://img.shields.io/github/stars/acestack-org/acestack-docs?style=flat-square&logo=github)](https://github.com/acestack-org/acestack-docs/stargazers)
[![Open issues](https://img.shields.io/github/issues/acestack-org/acestack-docs?style=flat-square&logo=github)](https://github.com/acestack-org/acestack-docs/issues)
[![Repo size](https://img.shields.io/github/repo-size/acestack-org/acestack-docs?style=flat-square&logo=github)](https://github.com/acestack-org/acestack-docs)

Source repository for **AceStack documentation**.
The site is statically generated with **Zensical** and published via **GitHub Pages**.

🌐 **Live site:**
[https://acestack-org.github.io/acestack-docs/](https://acestack-org.github.io/acestack-docs/)


## Getting started

### 1. Clone the repo

```bash
git clone https://github.com/acestack-org/acestack-docs.git
cd acostack-docs
```

### 2. Create an isolated runtime

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install zensical
```

### 3. Run the site locally

```bash
zensical serve
```

The site will be available at the URL shown in the console output.

## Contributing

1. Fork or branch from `main`.
2. Update files in `docs/` or tweak configuration under `overrides/`, `files/`, or the root.
3. Run `zensical serve` to preview your changes.
4. Commit with a descriptive message and open a pull request against `main`. Link to any local preview steps if the change requires manual verification.

## Project layout

- `docs/` – source Markdown, metadata, and layouts for the site.
- `overrides/` & `files/` – themed overrides, assets, and supporting media.
- `site/` – generated output (ignored by Git).
- `.cache/` – Zensical’s build cache.

## Notes

* `site/` is generated automatically and must **not** be committed.
* `.cache/` can be safely deleted if you run into build issues.
