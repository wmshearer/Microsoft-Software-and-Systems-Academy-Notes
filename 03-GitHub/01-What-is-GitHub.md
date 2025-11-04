# 🧭 What is GitHub?

GitHub is a **cloud-based collaboration platform** built on top of Git.  
It helps developers store, share, and track changes in their projects — all while working together efficiently.

---

## 🧩 Key Learning Objectives

- [x] Understand what GitHub is and what it’s built on.
- [x] Learn about repositories and how to create them.
- [x] Learn how to add and clone files.
- [x] Get familiar with Gists, Wikis, and GitHub Pages.

---

## 🧠 What is GitHub?

GitHub builds on **Git**, which is a distributed version control system that tracks file changes over time.

GitHub adds:
- 🌐 A **web interface** for managing Git projects.
- 🧑‍🤝‍🧑 **Collaboration tools** for teams.
- ⚙️ **Automation** with Actions and CI/CD.
- 🤖 **AI features** with GitHub Copilot.
- 🛡️ **Security tools** like Dependabot and secret scanning.

---

## 🧱 The Core Pillars of GitHub Enterprise

| Pillar | Description |
|--------|--------------|
| 🤖 **AI** | Uses tools like Copilot to boost developer productivity and assist with code. |
| 💬 **Collaboration** | Tools like Repositories, Issues, and Pull Requests make teamwork easier. |
| ⚙️ **Productivity** | Automate builds, tests, and deployments with GitHub Actions. |
| 🔐 **Security** | Features like Dependabot and CodeQL keep code secure and up to date. |
| 🌍 **Scale** | Over 100M developers and 420M+ repositories make GitHub the world’s largest dev platform. |

---

## 📁 What is a Repository?

A **repository (repo)** stores:
- All project files
- Each file’s change history
- Branches for testing and collaboration

Repos are where your project lives — they’re your **digital workspace**.

---

## ✅ Create a Repository on GitHub

- [ ] Go to [GitHub.com](https://github.com)
- [ ] Click the **+** icon (top right) → select **New repository**
- [ ] Choose an **Owner** (your username)
- [ ] Enter a **Repository name**
- [ ] Add a short **Description** *(optional)*
- [ ] Choose visibility:  
  - 🔓 **Public** → anyone can see it  
  - 🔒 **Private** → only invited users can
- [ ] Click **Create repository**
- [x] Repository created successfully 🎉

---

## 🖥️ Clone a Repository (Make a Local Copy)

- [ ] Go to your repo’s main page on GitHub
- [ ] Click the green **Code** button
- [ ] Copy the **HTTPS** URL
- [ ] Open your terminal (or Git Bash)
- [ ] Navigate to the folder where you want the copy  
  `cd path/to/your/folder`
- [ ] Run this command:
  ```bash
  git clone <repository-url>
