# 🚀 Push to GitHub (VS Code GUI + Beginner Safe Guide)

> 💡 If this is your first time publishing, **follow each checkbox**.  
> Once your repo is linked, future pushes are just:  
> `Edit → Save → + Stage → ✔ Commit → Push`.

---

## 1️⃣ Open Source Control
- [ ] Open VS Code  
- [ ] Click the **🌿 Source Control** icon on the left  
- [ ] Files appear under **Changes**

---

## 2️⃣ Initialize Repository (First Time Only)
- [ ] Click **Initialize Repository** in the Source Control panel  
- [ ] Files appear under **Changes** again (now Git is tracking them)

---

## 3️⃣ Stage & Commit (GUI)
- [ ] Hover over **Changes** → click the **+ (plus)** icon to stage all files  
- [ ] Type a short **commit message** at the top (example below)  
- [ ] Click the **✔ Commit** button  

**Example message:**
This is a test 123


### 🧠 Note
If a file called `COMMIT_EDITMSG` opens instead:
- Type your message on the blank line
- Press **Ctrl + Enter** to save and close  
- To prevent that window next time:  
  - ⚙️ **Settings → search “Use Commit Input As Editor” → Uncheck**  
  - (This tells VS Code to use the little message box instead.)

---

## 4️⃣ Publish Your Repo to GitHub
- [ ] Click **Publish Branch** (or **… → Publish Branch**)  
- [ ] Choose **GitHub**  
- [ ] Choose **Public** or **Private** (your choice)  
- [ ] Click **Publish Repository**  

If prompted to **Authorize Git Credential Manager**, click the **green Authorize** button — this lets VS Code connect securely to GitHub.

---

## 5️⃣ Confirm It’s Online
- [ ] Click **“Open on GitHub”** (blue link that appears)  
- [ ] Verify your files show up in the repo  
- [ ] Check that the correct branch (`main`) is active  

---

## 6️⃣ If You See “Can’t Push Refs” or “Pull First”
This just means GitHub has a commit you don’t (like an auto-created README).  
Fix it easily:
- [ ] **🌿 Source Control → … → Pull** (choose **Merge** if asked)
- [ ] If there are merge conflicts:
  - Open each file → click **Accept Both Changes**
  - **Ctrl + S** to save  
  - **+ Stage** → **✔ Commit** (message: `Merge remote changes`)  
- [ ] Then **🌿 … → Push** again

---

## 7️⃣ If “Publishing…” Seems Stuck
- [ ] Dismiss the progress toast (ˇ → X)  
- [ ] **🌿 … → Push** again  
- [ ] If still stuck: **View → Output → Git/GitHub** to check messages  
- [ ] **Ctrl + Shift + P → Developer: Reload Window**, then **Push** again  

---

## 8️⃣ Fix: GH007 — Private Email Push Block 💥

> Example error:
> ```
> remote: error: GH007: Your push would publish a private email address.
> ```
> This means GitHub blocked your push to protect your **real email**.  
> You just need to use your GitHub **no-reply** email instead.

### 🔍 How to Find Your GitHub No-Reply Email
1. Go to **GitHub → Settings → Emails**  
2. Make sure **“Keep my email addresses private”** is checked ✅  
3. Copy your no-reply address. It looks like: 241231240+TESTUSERr@users.noreply.github.com

### ⚙️ Update Git with That Email
In VS Code’s terminal (**Ctrl + `**):

```bash
git config --global user.email "241811240+wmshearer@users.noreply.github.com"
git config --global user.name "wmshearer"

Check it worked:

git config user.email
git config user.name

🔄 Update the Author on Your Last Commit
git commit --amend --no-edit --reset-author

🚀 Push Again

If it’s your first push:

git push -u origin main


If you see “pull first”:

git pull --rebase origin main
git push