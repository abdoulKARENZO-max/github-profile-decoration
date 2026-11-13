<<<<<<< HEAD
Here is the complete, updated `README.md` guide. It includes a dedicated **Customization Checklist** explaining every single variable, link, username, theme, and line of text that needs to be changed when setting this up for a different GitHub account.

```markdown
# 🚀 Step-by-Step Guide: Building a Dynamic GitHub Profile README

Use this template and guide to set up an automated, dynamic GitHub profile README for any GitHub account without relying on unstable public servers.

---

## 📋 Quick Customization Checklist (For Different Accounts)

Before copying the code below for a new or different GitHub account, make sure to update these specific fields:

| What to Change | Where to Find It | Example / Format |
| :--- | :--- | :--- |
| **Repository Name** | GitHub New Repo Page | Must match the exact username (e.g., `JaneDoe-dev/JaneDoe-dev`) |
| **GitHub Username** | `update-stats.yml` (3 places) & `README.md` (1 place) | **Case-sensitive** (e.g., `JaneDoe-dev`, NOT `janedoe-dev`) |
| **Display Name & Bio** | Top of `README.md` | `Hi 👋, I'm Jane` + bullet points (`- 🔭`, `- 👯`, `- 📫`) |
| **Typing Animation Lines** | `lines=` parameter in Typing SVG URL | Separate lines with `;` and replace spaces with `+` (e.g., `Data+Scientist;Python+Developer`) |
| **Social & Contact Links** | `Connect With Me` section in `README.md` | LinkedIn URL, Portfolio URL, and `mailto:` email address |
| **Tech Stack Icons** | `Languages and Tools` section in `README.md` | Add or remove `<a href="..."><img src="..." /></a>` blocks |
| **Color Theme** | `update-stats.yml` & `README.md` paths | Change `tokyonight` to `dracula`, `radical`, `gruvbox`, `solarized-dark`, or `default` |

---

## Step 1: Create the Special Profile Repository

1. Log in to the target GitHub account and create a **new public repository**.
2. Name the repository **identically** to the account's exact GitHub username (e.g., if the username is `JaneDoe-dev`, name the repo `JaneDoe-dev`).
3. Check **Add a README file** and click **Create repository**.
4. *(Optional)* To include private repository contributions in the stats and graphs, go to **GitHub Profile → Settings → Public profile** (or Contribution settings on the profile page) and check **Include private contributions on my profile**.

---

## Step 2: Set Up Automated SVG Generation (`.github/workflows/update-stats.yml`)

Create a new file in the repository at `.github/workflows/update-stats.yml` and paste the following code. **Replace all 3 instances of `TARGET_GITHUB_USERNAME`** with the exact case-sensitive username:

```yaml
name: Generate GitHub Profile Cards

on:
  schedule:
    - cron: "0 0 * * *" # Runs automatically every day at midnight UTC
  workflow_dispatch: # Allows manual triggering from the Actions tab

jobs:
  generate:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6

      # 1. Generates Profile Details, Stats, and Top Languages locally
      - name: Generate GitHub Profile Summary Cards
        uses: vn7n24fzkq/github-profile-summary-cards@release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          USERNAME: TARGET_GITHUB_USERNAME
          THEME: tokyonight # Options: tokyonight, dracula, radical, gruvbox, default

      # 2. Generates the Contribution Activity Graph locally
      - name: Generate Contribution Activity Graph SVG
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: TARGET_GITHUB_USERNAME
          outputs: |
            profile-cards/activity-graph.svg?palette=github-dark

      # 3. Caches GitHub Trophies locally with fallback protection
      - name: Cache GitHub Trophies SVG
        run: |
          mkdir -p profile-cards
          curl -s "[https://github-trophies.vercel.app/?username=TARGET_GITHUB_USERNAME&theme=tokyonight&no-frame=true&no-bg=true&margin-w=4&column=7](https://github-trophies.vercel.app/?username=TARGET_GITHUB_USERNAME&theme=tokyonight&no-frame=true&no-bg=true&margin-w=4&column=7)" -o profile-cards/trophies.svg
          if ! grep -q "<svg" profile-cards/trophies.svg; then
            curl -s "[https://github-profile-trophy.vercel.app/?username=TARGET_GITHUB_USERNAME&theme=tokyonight&no-frame=true&no-bg=true&margin-w=4&column=7](https://github-profile-trophy.vercel.app/?username=TARGET_GITHUB_USERNAME&theme=tokyonight&no-frame=true&no-bg=true&margin-w=4&column=7)" -o profile-cards/trophies.svg
          fi
          if ! grep -q "<svg" profile-cards/trophies.svg; then
            git checkout -- profile-cards/trophies.svg 2>/dev/null || rm -f profile-cards/trophies.svg
          fi

      # 4. Commits and pushes the generated SVGs to the repository
      - name: Commit and Push generated cards
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add .
          git diff --staged --quiet || git commit -m "Update profile stats, trophies, and activity graph"
          git push

```

### Trigger the Workflow First:

1. Go to the **Actions** tab in the repository.
2. Select **Generate GitHub Profile Cards** on the left sidebar.
3. Click **Run workflow** → **Run workflow**.
4. Wait ~30 seconds until it turns green and creates the `profile-cards/` and `profile-summary-card-output/` folders.

---

## Step 3: Populate `README.md`

Replace the contents of `README.md` with the template below, updating the placeholders marked with `TARGET_...`:

```markdown
=======
>>>>>>> 9948a47a23ca251aeadb1eafdda893885e7b6853
<h1 align="center">Hi 👋, I'm TARGET_DISPLAY_NAME</h1>

<!-- Customize roles in the 'lines=' parameter: separate lines with ';' and use '+' for spaces -->
<p align="center">
  <img src="[https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&duration=3000&pause=1000&color=36BCF7¢er=true&vCenter=true&width=700&lines=Role+One+Here;Role+Two+Here;Role+Three+Here](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&duration=3000&pause=1000&color=36BCF7¢er=true&vCenter=true&width=700&lines=Role+One+Here;Role+Two+Here;Role+Three+Here)" alt="Typing SVG" />
</p>

- 🔭 I’m currently working on **TARGET_CURRENT_PROJECTS**
- 👯 I worked on **TARGET_PAST_EXPERIENCE**
- 📫 How to reach me: **TARGET_EMAIL**

---

### 🌐 Connect With Me

<p align="left">
  <a href="TARGET_LINKEDIN_URL" target="_blank">
    <img src="[https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)" alt="LinkedIn" />
  </a>
  <a href="mailto:TARGET_EMAIL">
    <img src="[https://img.shields.io/badge/Email-Contact_Me-D14836?style=for-the-badge&logo=gmail&logoColor=white](https://img.shields.io/badge/Email-Contact_Me-D14836?style=for-the-badge&logo=gmail&logoColor=white)" alt="Email" />
  </a>
</p>

---

### 🛠️ Languages and Tools

<!-- Add or remove tool icons from devicon.dev or vectorlogo.zone -->
<p align="left">
  <a href="[https://www.python.org](https://www.python.org)" target="_blank" rel="noreferrer"> <img src="[https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg](https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg)" alt="python" width="40" height="40"/> </a>
  <a href="[https://developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)" target="_blank" rel="noreferrer"> <img src="[https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg](https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg)" alt="javascript" width="40" height="40"/> </a>
  <a href="[https://www.docker.com/](https://www.docker.com/)" target="_blank" rel="noreferrer"> <img src="[https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg](https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg)" alt="docker" width="40" height="40"/> </a>
  <a href="[https://kubernetes.io](https://kubernetes.io)" target="_blank" rel="noreferrer"> <img src="[https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg](https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg)" alt="kubernetes" width="40" height="40"/> </a>
  <a href="[https://aws.amazon.com](https://aws.amazon.com)" target="_blank" rel="noreferrer"> <img src="[https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg](https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg)" alt="aws" width="40" height="40"/> </a>
</p>

---

### 📊 GitHub Stats & Streaks

<!-- Note: If you changed THEME in update-stats.yml, update 'tokyonight' in the folder paths below to match -->
<p align="center">
  <img src="./profile-summary-card-output/tokyonight/0-profile-details.svg" alt="Profile Details" width="100%" />
</p>

<p align="center">
  <img src="./profile-summary-card-output/tokyonight/3-stats.svg" alt="GitHub Stats" width="48%" />
  <img src="./profile-summary-card-output/tokyonight/2-most-commit-language.svg" alt="Top Languages" width="48%" />
</p>

<p align="center">
  <img src="[https://streak-stats.demolab.com?user=TARGET_GITHUB_USERNAME&theme=tokyonight](https://streak-stats.demolab.com?user=TARGET_GITHUB_USERNAME&theme=tokyonight)" alt="GitHub Streak" width="55%" />
</p>

---

### 🏆 GitHub Trophies

<p align="center">
  <img src="./profile-cards/trophies.svg" alt="GitHub Trophies" width="100%" />
</p>

---

### 📈 Activity Graph

<p align="center">
  <img src="./profile-cards/activity-graph.svg" alt="Activity Graph" width="100%" />
<<<<<<< HEAD
</p>

```

---

## ⚠️ Maintenance Notes for Different Accounts

* **Inactivity Pause:** GitHub automatically disables scheduled (`cron`) workflows if a repository has no commits for **60 days**. If an account stays inactive for over two months, go to **Actions → Generate GitHub Profile Cards** and click **Enable workflow** / **Run workflow**.
* **Customizing Typing SVG Colors & Fonts:** You can visually customize the typing banner URL (font family, typing speed, hex color, and text lines) at [readme-typing-svg.demolab.com/demo](https://readme-typing-svg.demolab.com/demo/) and paste the generated `<img>` URL directly into `README.md`.
* **Theme Matching:** If you change `THEME: tokyonight` in `update-stats.yml` to another theme (such as `dracula`), GitHub Actions will output the files into `./profile-summary-card-output/dracula/...`. Be sure to update the folder name in your `README.md` image paths to match.

```

```
=======
</p>
>>>>>>> 9948a47a23ca251aeadb1eafdda893885e7b6853
