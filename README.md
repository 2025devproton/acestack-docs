# AceStack Docs

This repository contains the source code for the **AceStack documentation**.

📍 **Live site**: [https://acestack-org.github.io/acestack-docs/](https://acestack-org.github.io/acestack-docs/)

---

## Contributing to the documentation

### 1. Clone the repository

```bash
git clone https://github.com/acestack-org/acestack-docs.git
cd acostack-docs
```

---

### 2. Install Zensical

Zensical is used to build and serve the documentation locally.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install zensical
```

> ⚠️ Make sure you are using **Python 3.9+**.

---

### 3. Run the documentation locally

Zensical provides a local server, but **it does not support hot reload**.

You can run it in two different ways:

---

#### Option A — Standard mode

Starts the local documentation server. Any change requires a **manual restart**.

```bash
zensical serve --dev-addr localhost:5555
```

---

#### Option B — Auto-restart on file changes (recommended for development)

Although Zensical does not provide hot reload, you can configure an **automatic server restart** when files change using `nodemon`.

This significantly improves the development experience when editing documentation.

##### Requirements

- Node.js
- `nodemon`

Install globally:

```bash
npm install -g nodemon
```

##### Run with auto-restart

```bash
nodemon \
  --legacy-watch \
  --watch docs \
  --ext js,ts,json,yaml,yml,html,md \
  --ignore .zensical \
  --ignore dist \
  --exec "zensical serve --dev-addr localhost:5555"
```

This setup:

- Watches documentation files under `docs/`
- Automatically restarts the Zensical server on changes
- Ignores Zensical internal cache and build artifacts
- Uses `--legacy-watch` for better filesystem compatibility (Docker, WSL, network FS)

> ℹ️ This is **not hot reload**. You must refresh the browser manually after each restart.

---

### 4. Common issues and troubleshooting

#### Zensical fails to start after a crash or reload

Zensical keeps internal cache state that may become corrupted if the process is interrupted.

If this happens, stop the server and clean the generated directories:

```bash
rm -rf .cache site
```

Then start again:

```bash
zensical serve
```
