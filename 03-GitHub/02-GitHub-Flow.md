# ⚙️ GitHub Flow – Branches, Commits, and Pull Requests

The **GitHub Flow** is the step-by-step process developers use to make and share code changes safely.  
It’s built around three simple ideas:
1. **Branches** – work on your own copy without breaking the main project  
2. **Commits** – save your progress with a clear message  
3. **Pull Requests** – merge your work after review  

---

## 🧩 1. Create a Branch

A **branch** is your personal work area — like making a duplicate of the main project where you can test, fix, or add new features safely.

### 🖥️ Create a Branch in VS Code

1. Open VS Code.
   - From the top menu: **File → Open Folder...**
   - Choose your project folder (for example, `VSCode-GitHub-Guide`).
2. Open the **Source Control** panel:
   - Shortcut: **Ctrl + Shift + G**
   - OR click the **branch icon** on the left sidebar.
3. Look at the **bottom-left corner** of VS Code. You’ll see your current branch name (likely `main`).
4. Click that name → select **Create new branch...**
5. Enter a name like:
6. Press **Enter**.

✅ You’ve now created a new branch and automatically switched to it.  
Confirm in the bottom-left — it should say your new branch name.

---

## 🧠 2. Make and Commit a Change

### ✏️ Edit a File

1. Open any file (for example, `README.md`).
2. Add or change something small — even a test line.
3. Save your work:
- Shortcut: **Ctrl + S**
- OR Menu → **File → Save**

### 💾 Stage and Commit in VS Code

1. Open the **Source Control** panel (**Ctrl + Shift + G**).
2. You’ll see your changed file listed.
3. Hover over it and click the **+** icon to stage it.
4. In the message box at the top, write something clear like:

5. Click the **✔ Commit** button or press **Ctrl + Enter**.

✅ You’ve made a local commit!  
This means the change is saved in your Git history but not yet uploaded to GitHub.

---

## 💻 3. Push Your Branch to GitHub

Now we’ll send your branch (and commit) to GitHub.

### 🧭 Open Your Terminal

1. Open the terminal in VS Code:
- Shortcut: **Ctrl + `**
- OR Menu → **Terminal → New Terminal**
2. Make sure you’re using **Git Bash**:
- If not, click the dropdown arrow in the terminal tab and choose **Select Default Profile → Git Bash**
- Then click **+** to open a new Git Bash terminal.

---

## 🧱 4. Work in the Terminal (All in One Session)

Now you’ll run everything below in this single terminal window.

```bash
# Check which branch you're on
git status

# (Optional) Create and switch to a new branch if needed
git checkout -b feature-github-flow

# Stage all modified files
git add .

# Commit your changes with a message
git commit -m "Add intro to README"

# Push your branch to GitHub
git push -u origin feature-github-flow

# (After merging the PR on GitHub)
# Switch back to the main branch
git checkout main

# Pull down the latest updates from GitHub
git pull

# Delete the local branch to keep things clean
git branch -d feature-github-flow 
```
## 🌐 5. Open a Pull Request on GitHub.com

Now that your branch is on GitHub, you can open a Pull Request (PR).

Go to GitHub.com → your repository.

You’ll see a banner suggesting a pull request. Click Compare & pull request.

Add a short title and description of what you changed.

Make sure:

Base branch = main

Compare branch = your branch name (e.g., feature-github-flow)

Click Create Pull Request.

✅ Your PR is now live — this is where teammates can review or comment on your changes.

## 🔀 6. Review and Merge

### If you’re the only person working on this repo:

Open the Pull Request on GitHub.

Click the Files changed tab to review what’s new.

Click Merge pull request → Confirm merge.

### If you’re working with others:

Assign reviewers using the Reviewers box on the right.

Wait for approval before clicking Merge pull request.

✅ Once merged, your changes are part of the main branch!

## 🧹 7. Clean Up (Locally & Remotely)

After merging, you can delete your branch to keep things tidy.

🧽 On GitHub.com

On your Pull Request page, click Delete branch.

🧽 In VS Code (Terminal)

Run these commands (already included above but here’s a reminder):

✅ You’re done with all local Git commands in one clean sequence.
```bash
git checkout main
git pull
git branch -d feature-github-flow
```

## ✅ Local cleanup done!

📋 Quick Summary Checklist
| Step | Task                     | Where                | Done |
| ---- | ------------------------ | -------------------- | ---- |
| 1    | Create a new branch      | VS Code              | ✅    |
| 2    | Edit a file              | VS Code              | ✅    |
| 3    | Stage and commit changes | VS Code              | ✅    |
| 4    | Push to GitHub           | Terminal             | ✅    |
| 5    | Open Pull Request        | GitHub.com           | ✅    |
| 6    | Merge into main          | GitHub.com           | ✅    |
| 7    | Delete branch            | GitHub.com + VS Code | ✅    |


## 🧾 Notes & Tips

💡 Branch naming: use short, clear names like feature-login, fix-typo, or update-readme.
💡 Commit messages: always start with a verb (e.g., “Add”, “Fix”, “Update”).
💡 If you get errors pushing: make sure you’re logged into GitHub inside VS Code (bottom left → “Sign in to GitHub”).