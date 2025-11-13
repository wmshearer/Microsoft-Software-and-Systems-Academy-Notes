# 🌿 05 – Branching and Merging Practice

This guide walks you through **creating a new branch**, making changes, committing, pushing, and merging into `main`.  
It includes **VS Code (GUI)** and **Terminal** methods side by side for clarity.

---

## 🧩 What You’ll Learn
- Create and switch branches  
- Add and commit files  
- Push your branch to GitHub  
- Open and merge a Pull Request  
- Clean up branches after merging  

---

## ✅ Step 1: Make Sure Your Repository Is Up to Date

### VS Code GUI
1. Open your project folder in **VS Code**.  
2. Click the **Source Control** icon (📦 on the left sidebar).  
3. Click **Sync Changes** (🔁) if available.  
4. Confirm you’re on the `main` branch (bottom-left corner).

### Terminal (inside VS Code or Git Bash)
```bash
git checkout main
git pull origin main
```

---

## ✅ Step 2: Create a New Branch

### VS Code GUI
1. Look at the **bottom-left** corner of VS Code — it shows your current branch (e.g., `main`).  
2. Click that branch name → select **Create new branch…**  
3. Name your branch something descriptive, like:
   ```bash
   feature/test-branch
   ```

---

## ✅ Step 3: Create a File and Make a Change

### VS Code GUI
1. In the **Explorer panel** (top-left), right-click your repo folder → **New File**  
2. Name it:
   ```bash
   test-branch-notes.md
   ```
3. Add the following content:
   ```markdown
   ### Test Branch Notes
   This file was created to practice branching and merging in GitHub.
   ```
4. Save your file → **Ctrl + S** (Windows) or **Cmd + S** (Mac).

---

## ✅ Step 4: Stage and Commit Your Changes

### VS Code GUI
1. Click the **Source Control** icon (branch symbol on the left).  
2. Hover over your new file and click the **+** to stage it.  
3. Type a commit message, for example:
   ```
   Add test-branch-notes.md for branch testing
   ```
4. Click the **Commit (✔️)** button or press **Ctrl + Enter**.

### Terminal
```bash
git add test-branch-notes.md
git commit -m "Add test-branch-notes.md for branch testing"
```

---

## ✅ Step 5: Push Your Branch to GitHub

### VS Code GUI
- In the **Source Control** panel, click **Publish Branch** (or **Sync Changes** if shown).  
- VS Code will upload your new branch to GitHub automatically.

### Terminal
```bash
git push -u origin feature/test-branch
```

---

## ✅ Step 6: Create a Pull Request (PR)

### On GitHub.com
1. Go to your **repository page**.  
2. You should see a yellow banner:
   > “feature/test-branch had recent pushes… Compare & pull request.”  
3. Click **Compare & pull request**.  
4. Make sure:
   - **base branch:** `main`  
   - **compare branch:** `feature/test-branch`  
5. Add a title like:
   ```
   Add test-branch-notes.md for practice
   ```
6. Click **Create Pull Request**.

---

## ✅ Step 7: Merge the Branch

1. Review the file changes shown.  
2. Click **Merge Pull Request** → **Confirm Merge**.  
3. You’ll see:
   > “Pull request successfully merged and closed.” 🎉

---

## ✅ Step 8: Clean Up

### On GitHub
- Click **Delete branch** (it appears right after merging).

### Locally (VS Code or Terminal)
```bash
git checkout main
git pull origin main
git branch -d feature/test-branch
```

💡 **Why Delete Branches?**  
Deleting merged branches keeps your project tidy and avoids confusion later.  
Your work is already stored in `main` after merging — no data is lost.

---

## 🧾 Quick Summary Checklist

| Task | Description |
|------|--------------|
| 🔁 | Switch to `main` and pull latest changes |
| 🌿 | Create new branch (GUI or terminal) |
| 📝 | Make and save edits |
| 📦 | Stage and commit changes |
| 🚀 | Push branch to GitHub |
| 🔃 | Open a Pull Request |
| 🔀 | Merge into `main` |
| 🧹 | Delete branch (local + remote) |

---

✅ **You did it!**  
You’ve now practiced the **full Git branching workflow** — from creation to merge cleanup.  
This is the foundation of real-world team collaboration on GitHub. 🌱
