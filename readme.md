# Syncing iOS Localization Gaps with Google Sheets and GitHub PR Placeholders

This n8n workflow automatically identifies missing translations in `.strings` files across iOS localizations (e.g., `Base.lproj` vs `fr.lproj`) and generates a report in **Google Sheets**. Optionally, it creates a **GitHub Pull Request** to insert placeholder strings (`"__TODO_TRANSLATE__"`) so builds don’t fail. Supports **DRY_RUN** mode. :contentReference[oaicite:1]{index=1}

---

## 🚀 Quick Start – Setup Steps

1. **Import the Workflow JSON** into your n8n instance. :contentReference[oaicite:2]{index=2}
2. **Set Config Node values**, for example:
   ````json
   {
     "GITHUB_OWNER": "your-github-user-name",
     "GITHUB_REPO": "your-iOS-repo-name",
     "BASE_BRANCH": "develop",
     "SHEET_ID": "<YOUR_GOOGLE_SHEET_ID>",
     "ENABLE_PR": "true",
     "IOS_SOURCE_GLOB": "**/Base.lproj/*.strings,**/en.lproj/*.strings",
     "IOS_TARGET_GLOB": "**/*.lproj/*.strings",
     "PLACEHOLDER_VALUE": "__TODO_TRANSLATE__",
     "BRANCH_TEMPLATE": "chore/l10n-gap-{{YYYYMMDD}}"
   }
   ``` :contentReference[oaicite:3]{index=3}
   ````

---

## 📌 What It Does

- Listens for GitHub push or pull request events.
- Scans the iOS repository for `.strings` files under source and target language directories.
- Compares translation keys between source and target to identify missing entries.
- Updates or creates Google Sheet tabs (e.g., `fr`) with missing entries.
- Optionally creates a **GitHub PR** with placeholder values so missing translations don’t break builds.
- Supports **DRY_RUN** mode (sheet only, no PR). :contentReference[oaicite:4]{index=4}

---

## 👥 Who’s It For

- iOS development teams who want fast feedback on missing localizations.
- Localization managers who want a shared sheet to assign work to translators. :contentReference[oaicite:5]{index=5}

---

## 🛠 Requirements

| Tool                 | Purpose              | Notes                               |
| -------------------- | -------------------- | ----------------------------------- | ------------------------------------- |
| **GitHub Repo**      | Webhook, API for PRs | Requires `repo` token or App access |
| **Google Sheets**    | Sheet output         | Needs valid `SHEET_ID`              |
| **Slack (optional)** | Notifications        | Requires `chat:write` scope         |
| **SMTP (optional)**  | Email fallback       | Standard SMTP credentials           | :contentReference[oaicite:6]{index=6} |

---

## ⚙️ How It Works (Architecture)

1. **GitHub Webhook Trigger** — Fired on push or pull request.
2. **Scan Repository Tree** — Finds `.strings` source and target files based on glob patterns.
3. **Key Comparison** — Detects missing translation keys.
4. **Update Sheets** — Adds missing entries to the appropriate sheet tab.
5. **(Optional) Create GitHub PR** — Opens a pull request with placeholder translations. :contentReference[oaicite:7]{index=7}

---

## 🛠 How to Customize

- **Multiple Locales** — Add comma‑separated values to `TARGET_LANGS_CSV` (e.g., `fr,de,es`).
- **Glob Patterns** — Adjust `IOS_SOURCE_GLOB` and `IOS_TARGET_GLOB` to scan only certain modules.
- **Ignore Rules** — Add `IGNORE_KEY_PREFIXES_CSV` to skip internal or debug strings.
- **Placeholder Value** — Change `PLACEHOLDER_VALUE` to something meaningful like `"@@@"`.
- **Slack/Email** — Set `SLACK_CHANNEL` and `EMAIL_FALLBACK_TO_CSV`.
- **DRY_RUN Mode** — Set to `true` to update Sheets only without PR creation. :contentReference[oaicite:8]{index=8}

---

## ➕ Add‑Ons & Extensions

- **Android support** — Add a path for `strings.xml` (`values` → `values‑<lang>`), diff → Sheets → placeholder PR.
- **Multiple languages** — Expand `TARGET_LANGS_CSV` and loop through tabs + placeholder commits per locale.
- **`.stringsdict` handling** — Validate plural/format entries and open precise PRs.
- **Translator DMs** — Map language → Slack handle/email to DM translators with their file/key counts.
- **GitLab/Bitbucket variants** — Replace GitHub API calls with equivalents to open merge requests. :contentReference[oaicite:9]{index=9}

---

## 📈 Use Case Examples

- Before a test build, ensure the locale (e.g., `fr`) has all keys present — placeholders keep the app compiling.
- Weekly runs create sheets for translators and PRs with placeholders, avoiding last‑minute breakages.
- When adding a new screen with new strings, the workflow flags missing keys across locales and pre‑fills them. :contentReference[oaicite:10]{index=10}

---

## 🧪 Common Troubleshooting

| **Issue**                | **Possible Cause**                                    | **Solution**                                |
| ------------------------ | ----------------------------------------------------- | ------------------------------------------- | --------------------------------------- |
| No source files found    | Glob pattern doesn’t match `Base.lproj` or `en.lproj` | Adjust `IOS_SOURCE_GLOB`                    |
| Target file missing      | Locale folder doesn’t exist yet                       | Placeholder PR will create it               |
| Parsing skips entries    | Non‑standard .strings format                          | Ensure proper `"key" = "value";` syntax     |
| Sheet not updating       | Invalid `SHEET_ID` or insufficient permission         | Provide a valid ID or allow write access    |
| PR not created           | `ENABLE_PR=false` or no keys missing                  | Enable PR creation and confirm missing gaps |
| Slack/Email not received | Missing credentials/config                            | Configure Slack/SMTP and set recipients     | :contentReference[oaicite:11]{index=11} |

---

## 💬 Need Help?

Want to expand this for Android? Loop through 5+ locales at once? Replace GitHub with GitLab?  
Contact the **WeblineIndia.n8n experts** with your repo and locale setup and we can tailor this workflow to your translation process! :contentReference[oaicite:12]{index=12}
