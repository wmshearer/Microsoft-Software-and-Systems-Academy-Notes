# 🔧 Hands-On: Full GitHub Flow (GUI-first, Windows 11)

You’ll practice the complete flow:
1) Create a feature branch
2) Edit a file
3) Stage + commit (GUI)
4) Push to GitHub
5) Open + merge a Pull Request
6) Clean up branches

---

## 🧭 Prep: Open your project in VS Code
- Open folder: **Menu Bar → File → Open Folder…** → select your repo folder.
- Toggle sidebar if hidden: **Ctrl + B**.
- Open Source Control: **Ctrl + Shift + G** or click **🌿** on the left.

---

## 1) Create a new branch (GUI)
- Bottom-left status bar → click branch name (likely `main`) → **Create new branch…**
- Name it (example): `feature-hands-on`
- Confirm you switched (bottom-left now shows `feature-hands-on`).

---

## 2) Make an edit (GUI)
- In **Explorer (📂)**, open a Markdown file (e.g., `README.md`), or create one (**New File (📄)**).
- Type a new line (example):  
  `- [ ] Hands-on step complete`
- Save: **Ctrl + S**.

---

## 3) Stage & commit (GUI)
- Open **🌿 Source Control** (**Ctrl + Shift + G**).
- Click **+** next to the changed file (stages it).
- In the message box, type:  
  `Add hands-on checklist item`
- Commit: click **✔ Commit** (or **Ctrl + Enter**).
  - If `COMMIT_EDITMSG` opens, type the message and press **Ctrl + Enter** to finish.
  - To avoid that editor in future: **⚙️ Settings → search “Use Commit Input As Editor” → uncheck**.

---

## 4) Push your branch to GitHub (GUI)
- **… (three dots)** → **Push**
- If prompted, **Publish Branch** → pick **GitHub**.
- If asked to authorize **Git Credential Manager**, click the green **Authorize** button in the browser.

> If push fails with **“pull first”**:
> - **… → Pull** (choose **Merge**), then **… → Push**.

> If push fails with **GH007 (private email)**:
> - Use GitHub noreply email (Settings → Emails). Example:  
>   `241811240+testuser@users.noreply.github.com`
> - Then run the single terminal session at the bottom (look for **GH007 fix**).

---

## 5) Open a Pull Request (GitHub.com)
- Click the **Open on GitHub** link in VS Code, or go to your repo on **GitHub.com**.
- You’ll see a prompt → **Compare & pull request**.
- Title: `Add hands-on checklist item`  
  Description: brief note of what changed.
- Base = `main`, Compare = `feature-hands-on`.
- **Create Pull Request**.

---

## 6) Merge & clean up
- On the PR page, **Merge pull request → Confirm merge**.
- Click **Delete branch** (removes remote branch).

Back in VS Code:
- **Ctrl + `** to open terminal (or **Terminal → New Terminal**).
- Run the **one** terminal session below to sync `main` and clean up locally.

---

```bash
# Make sure you're in the repo folder
# (If not, in VS Code: File → Open Folder… and select your repo)

# 1) Confirm current branch and changes
git status

# 2) (Optional) If GH007 email privacy blocked earlier, set noreply and amend last commit
#    Replace with your noreply if different
git config --global user.email "241811240+TESTUSER@users.noreply.github.com"
git config --global user.name "TESTUSER"
git commit --amend --no-edit --reset-author || echo "No amend needed or commit already ok"

# 3) Push current feature branch (first-time push sets upstream)
git push -u origin feature-hands-on || echo "If this asks to pull first, keep going"

# 4) If remote had changes (e.g., you merged the PR on GitHub), sync local main and clean up
git checkout main
git pull

# 5) Delete the local feature branch (safe after merge)
git branch -d feature-hands-on || echo "If not merged yet, skip deleting"

# 6) Verify everything is clean
git status
```
## 🧾 Quick Checklist

 Created feature-hands-on branch

 Edited a file and saved (Ctrl + S)

 Staged (+) and committed (✔)

 Pushed to GitHub

 Opened & merged Pull Request

 Ran the terminal block to sync main and delete local branch

 ## 🛠 Troubleshooting (fast)

No tracking info / Publish needed → Publish Branch or … → Branch → Set Upstream… → origin/main

“Pull first” → … → Pull (Merge) → then … → Push

GH007 → set noreply email + git commit --amend --no-edit --reset-author (see terminal block)

Spinner stuck → dismiss toast → … → Push → View → Output → Git/GitHub → Ctrl + Shift + P → Developer: Reload Window