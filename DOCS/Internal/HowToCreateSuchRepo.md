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
4. Add a label called `hero-app-request` in github repo via repo -> **Issues** -> **Labels** -> **New Label**


**Create the Issue Form UI:**
We use YAML to build a strict UI form so users can't mess up the formatting.
1. In the `hero-app-requests` repository, create a file at this exact path `.github/ISSUE_TEMPLATE/new-hero-app.yml`.
Or just copy the one from this repo.

---

### Phase 3: The Automation Engine

Now, we write the GitHub Action that listens for this form to be submitted, generates an authentication token using your App, and builds the repository.

1. In the same `hero-app-requests` repository, create a file at `.github/workflows/generator.yml`.
Or just copy the one from this repo.

---

### How this flows in reality:

1. A developer visits `blah-community-untrusted/hero-app-requests`.
2. They click "Issues" -> "New Issue". They see a beautiful, polished UI form asking for a name and description.
3. They submit it.
4. The Action wakes up, verifies the data, securely creates `hero-app-[their-name]`, shoots them an email invite to take control of it, and closes the request issue automatically.

All of this happens in about 15 seconds.
