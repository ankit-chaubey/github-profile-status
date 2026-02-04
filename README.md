# GitHub Profile Status

A **production‑ready GitHub profile status generator** that runs entirely on **GitHub Actions**.
It automatically fetches, normalizes, and persists public GitHub data (profile, repositories, languages, commits, organizations, and contribution graph) into structured JSON and SVG artifacts.

This repository is designed to act as a **data backend** for GitHub profile dashboards, personal websites, analytics tools, and automation workflows.

---

## ✨ Features

* ⚙️ **Fully automated via GitHub Actions** (no local execution required)
* 📦 Fetches **complete public repository data** with pagination
* 🧠 Handles GitHub quirks (202 responses for stats, retries, rate‑limit awareness)
* 🧾 Generates clean, reusable **JSON artifacts**
* 📊 Downloads GitHub **contribution graph (SVG)**
* 🔐 Uses `GITHUB_TOKEN` securely (no secrets leakage)
* 🚀 CI‑friendly with controlled concurrency

---

## 📂 Generated Output

All generated files are written to the `data/` directory:

| File           | Description                                                     |
| -------------- | --------------------------------------------------------------- |
| `profile.json` | Public GitHub user profile data                                 |
| `repos.json`   | Full list of public repositories with languages & latest commit |
| `orgs.json`    | Organizations the user belongs to                               |
| `summary.json` | Aggregated statistics (repos, stars, forks, commits, followers) |
| `contrib.svg`  | GitHub contribution graph (SVG)                                 |

These files are **committed back to the repository** by the workflow.

---

## 🛠 How It Works

1. A GitHub Actions workflow is triggered (manual or scheduled)
2. The Node.js script uses **Octokit** to fetch GitHub public data
3. Data is fetched with:

   * Pagination
   * Limited concurrency
   * Retry & polling logic (for stats endpoints)
4. Results are normalized and saved as JSON/SVG
5. Changes are committed back to the repository automatically

---

## 🚀 Usage

### Run via GitHub Actions (recommended)

The repository is designed to run **only via CI**.

You can trigger it using:

* `workflow_dispatch` (manual run)
* `schedule` (cron‑based automation)

The workflow automatically uses:

```bash
${{ secrets.GITHUB_TOKEN }}
```

No additional setup is required.

---

## ⚙️ Configuration

The generator supports configuration via **environment variables / flags**:

| Option              | Description                                  |
| ------------------- | -------------------------------------------- |
| `--user=<username>` | GitHub username to fetch data for            |
| `--token=<token>`   | GitHub token (PAT or `GITHUB_TOKEN`)         |
| `FETCH_STATS=false` | Disable contributor stats for faster CI runs |

Example (inside GitHub Actions):

```bash
node scripts/generate.js \
  --user=${{ github.repository_owner }} \
  --token=${{ secrets.GITHUB_TOKEN }}
```

---

## 🧠 Why This Exists

GitHub exposes powerful APIs, but:

* Data is fragmented
* Stats endpoints are slow or unstable
* Manual fetching doesn’t scale

This project solves that by providing a **repeatable, CI‑driven data pipeline** that keeps GitHub profile data **always up‑to‑date**.

---

## 📌 Use Cases

* GitHub profile dashboards
* Personal portfolio websites
* Contribution analytics
* Resume / activity metrics
* GitHub profile automation

---

## 🧱 Tech Stack

* **Node.js**
* **Octokit (GitHub REST API)**
* **GitHub Actions**
* **node-fetch**

---

## 🔒 Security

* Uses GitHub‑provided `GITHUB_TOKEN`
* No private data access
* No token logging
* Minimal permissions (`contents: write`)

---

## 📜 License

MIT License

---

## 👤 Author

**Ankit Chaubey**
GitHub: [https://github.com/ankit-chaubey](https://github.com/ankit-chaubey)

---

> This repository is intentionally designed as a **data engine**, not a UI.
> Feel free to consume the generated artifacts in any frontend, dashboard, or automation pipeline.
