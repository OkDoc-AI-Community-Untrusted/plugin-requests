Here is your step-by-step guide to setting up the automated hero app generator for `blah-community-untrusted`.

---

### Phase 1: Secure Authentication (The GitHub App)

We need to give your automation the permission to create repositories and invite users, scoped *only* to this specific organization.

1. Go to your organization settings: `https://github.com/organizations/blah-community-untrusted/settings/profile`
2. Scroll down to **Developer settings** (bottom left) -> **GitHub Apps** -> **New GitHub App**.
3. Name it something like "Blah Hero App Automator".
4. Enter your website URL (required, but can just be your main site).
5. **CRITICAL:** Uncheck the "Active" box under Webhooks. We don't need webhooks since we are using GitHub Actions.
6. Under **Repository permissions**, grant:
   * **Administration:** `Read & write` (to create the repo).
   * **Issues:** `Read & write` (to access the Issues section).

7. Under **Organization permissions**, grant:
   * **Members:** `Read & write` (to invite the user as an outside collaborator).

8. Click **Create GitHub App**.
9. On the next screen, click **Generate a private key**. A `.pem` file will download to your computer. Keep this safe.
10. Finally, click **Install App** (left sidebar) and install it on the `blah-community-untrusted` organization.

---

### Phase 2: The Request Repository & Issue Form

Now we create the public-facing place where users actually ask for a repository.

1. Inside `blah-community-untrusted`, create a new public repository named `hero-app-requests`.
2. Go to the Settings of this new `hero-app-requests` repo -> **Secrets and variables** -> **Actions**.
3. Create two **New repository secrets**:
   * `APP_ID`: The App ID from your GitHub App settings (a number).
   * `APP_PRIVATE_KEY`: Paste the entire contents of the `.pem` file you downloaded earlier.

4. Under **Secrets and variables** -> **Actions** -> **Variables** tab, create three **repository variables**:

   | Variable | Purpose | Example value |
   |---|---|---|
   | `REPO_SUFFIX` | Suffix appended to every created repo name | `-hero-app` |
   | `REPO_TITLE_SUFFIX` | Human-readable suffix used in descriptions and titles | `Hero App` |
   | `GRANT_MAINTAINER_ACCESS` | `true` to invite the requester as a Maintainer, `false` to skip (they will use the fork workflow instead) | `true` |

   > These variables are read by the workflow at runtime. Changing them updates the behavior for all future requests — no code changes needed.

5. Add a label called `hero-app-request` in github repo via repo -> **Issues** -> **Labels** -> **New Label**


**Create the Issue Form UI:**
We use YAML to build a strict UI form so users can't mess up the formatting.
1. In the `hero-app-requests` repository, create a file at this exact path `.github/ISSUE_TEMPLATE/new-hero-app.yml`.
   Or just copy the one from this repo.

> The issue form warns users **not** to include "hero" or "hero-app" in the name. The suffix is added automatically by the workflow.

---

### Phase 3: The Automation Engine

Now, we write the GitHub Action that listens for this form to be submitted, generates an authentication token using your App, and builds the repository.

1. In the same `hero-app-requests` repository, create a file at `.github/workflows/generator.yml`.
   Or just copy the one from this repo.

**What the workflow does:**
1. Parses the name and description from the issue body.
2. **Validates** — rejects names containing "hero" or "hero-app" (closes the issue with an explanatory comment).
3. Appends the `REPO_SUFFIX` variable to produce the final repo name (e.g. `my-tool` → `my-tool-hero-app`).
4. Creates the repo in the organization.
5. Checks `GRANT_MAINTAINER_ACCESS`:
   - **`true`** → invites the requester as Maintainer and posts a success message.
   - **`false`** → posts a message with fork & PR instructions so the requester knows how to contribute.
6. Closes the issue.

---

### How this flows in reality:

1. A developer visits `blah-community-untrusted/hero-app-requests`.
2. They click "Issues" → "New Issue". They see a polished UI form asking for a name and description.
3. They submit it.
4. The Action wakes up, validates the name, appends the suffix to create `[their-name]-hero-app`, optionally invites them as Maintainer, posts a comment with next steps, and closes the issue automatically.

All of this happens in about 15 seconds.
