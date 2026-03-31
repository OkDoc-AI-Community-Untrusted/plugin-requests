# OkDoc.AI Community Hero App Requests

Welcome! This repository is where you request a new **Hero App** repository in the OkDoc.AI community organization.

---

## How It Works

1. You submit a request via a GitHub Issue form.
2. An automated workflow creates the repository for you.
3. Depending on organization settings you either receive **Maintainer access** directly, or you contribute through the **Fork & Pull Request** workflow.

---

## Step 1 — Submit a Request

1. Go to the **Issues** tab of this repository.
2. Click **New Issue** and select **🚀 Request a OkDoc.AI Community Hero App Repository**.
3. Fill in the form:
   - **Desired Repository Name** — lowercase letters and hyphens only (e.g. `video-player`, `smart-scheduler`).
   - **Short Description** — a brief summary of what your hero app does.
4. Click **Submit new issue**.

### Naming Rules

| Rule | Example |
|------|---------|
| Lowercase letters and hyphens only | `my-cool-tool` ✅ &nbsp; `My_Cool Tool` ❌ |
| **Do NOT include "hero", "Hero App", or "hero-app"** in the name — the suffix is appended automatically | `my-tool` ✅ → creates `my-tool-hero-app` |
| Keep it short and descriptive | `voice-transcriber` ✅ |

> If your name contains "hero" or "hero-app" the request will be **automatically rejected** with a comment explaining why.

---

## Step 2 — After the Repository Is Created

Once the automation finishes (usually within seconds), a comment will be posted on your issue with a link to the new repository.

### If you received Maintainer access

Check your email or GitHub notifications for the invitation. Accept it and you can push code directly:

```bash
git clone https://github.com/ORG_NAME/your-tool-hero-app.git
cd your-tool-hero-app
# make your changes
git add . && git commit -m "Initial commit"
git push origin main
```

### If direct access is not enabled (Fork workflow)

You will contribute through the standard open-source fork model:

1. **Fork** the repository — click the **Fork** button on the repo page.
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/your-tool-hero-app.git
   cd your-tool-hero-app
   ```
3. **Create a feature branch** and make your changes:
   ```bash
   git checkout -b my-feature
   # ... write your code ...
   git add . && git commit -m "Add initial hero app code"
   git push origin my-feature
   ```
4. **Open a Pull Request** from your fork back to the original repository.
5. A maintainer will review and merge your changes.

> **Need direct access later?** Open a new issue in this repository requesting Maintainer access to your repo.

---

## Rules & Guidelines

- One request per hero app. Do not submit duplicate requests.
- Repository names must be unique within the organization.
- Keep descriptions accurate — they appear in the repo's About section.
- Follow the organization's code of conduct and contribution guidelines.

---

## Questions?

If something went wrong with your request or you have questions, open a regular issue (not the hero app form) and a maintainer will help you out.
