<p align="center">
  <img src="https://img.shields.io/badge/n8n-Workflow%20Automation-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-Self--Hosted-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Gmail-API%20Integration-EA4335?style=for-the-badge&logo=gmail&logoColor=white" />
  <img src="https://img.shields.io/badge/OAuth2-Authenticated-4285F4?style=for-the-badge&logo=google&logoColor=white" />
</p>

# 🚀 Automated Gmail Auto-Responder — n8n Workflow

> **A production-ready, keyword-triggered email auto-responder** built with n8n, deployed on both **n8n Cloud** and a **self-hosted Docker environment** to demonstrate full-stack automation capabilities.

---

## 📌 Project Overview

This project implements a fully automated email auto-responder using **[n8n](https://n8n.io/)** — an open-source workflow automation tool. The system continuously monitors a Gmail inbox for new messages. When an incoming email contains the keyword **"KHIZRU"**, the workflow:

1. ✅ **Detects** the keyword in the email body or subject (case-insensitive)
2. ✉️ **Replies** to the sender in the same thread with `"HI KHIZRU HERE"`
3. 📬 **Marks** the original email as read to prevent re-processing and infinite loops

To showcase versatility across deployment environments, the workflow was:
- **Prototyped & tested** on [n8n Cloud](https://app.n8n.cloud) using the built-in AI Assistant
- **Migrated & deployed** to a local self-hosted Docker instance

---

## ⚙️ Workflow Architecture

The automation pipeline consists of **four sequential nodes**:

```
┌──────────────────┐    ┌───────────────────┐    ┌──────────────────┐    ┌─────────────────────┐
│  Gmail Trigger   │───▶│  Filter: Contains │───▶│  Gmail: Reply    │───▶│  Gmail: Mark as     │
│  (Every Minute)  │    │  "KHIZRU"?        │    │  to Sender       │    │  Read               │
└──────────────────┘    └───────────────────┘    └──────────────────┘    └─────────────────────┘
```

| Node | Type | Description |
|------|------|-------------|
| **New Incoming Email** | `Gmail Trigger` | Polls the inbox every minute for new, unread emails (excludes self-sent messages) |
| **Contains KHIZRU?** | `If` (Filter) | Evaluates whether the email body/subject contains `"KHIZRU"` (case-insensitive) |
| **Reply to Sender** | `Gmail` (Reply) | Sends a threaded reply with the message `"HI KHIZRU HERE"` |
| **Mark Email as Read** | `Gmail` (Mark as Read) | Marks the processed email as read to prevent the trigger from re-processing it |

---

## 📸 Proof of Concept

### n8n Cloud — Workflow Canvas
> The workflow running on n8n Cloud at `n8n-mkhizar.app.n8n.cloud`, showing all four nodes in the published state.

![n8n Cloud Workflow Canvas](Results/Cloud/Web.png)

---

### Self-Hosted Docker — Workflow Canvas
> The same workflow imported and running on a local Docker instance at `localhost:5678`.

![Docker Self-Hosted Workflow Canvas](Results/Docker/Image.png)

---

### Gmail — Test Email (Sender's View)
> The sender's Gmail inbox showing the original email with the keyword `"KHIZRU"` sent from an ITU email address (`bsce22042@itu.edu.pk`), and the automated reply `"HI KHIZRU HERE"` received instantly.

![Gmail Test - Sender View](Results/Test_Sender.png)

---

### Gmail — Test Email (Receiver's View)
> The receiver's Gmail inbox confirming the threaded reply `"HI KHIZRU HERE"` was delivered successfully and automatically.

![Gmail Test - Reply View](Results/Test_Reply.png)

---

## 🛠️ Step-by-Step Setup & Deployment

### Part 1: Prototyping on n8n Cloud

1. Created a free trial account on [n8n Cloud](https://app.n8n.cloud).
2. Utilized the **n8n AI Assistant** to generate the initial workflow structure using a natural language prompt.
3. Connected **Gmail API** credentials via **OAuth2** to enable live trigger and reply functionality.
4. Tested the workflow end-to-end by sending test emails containing the keyword.
5. Exported the finalized workflow as a [`.json` file](Ice-Breaking%20Session.json) for version control and portability.

### Part 2: Self-Hosting Locally via Docker

To demonstrate self-hosting capabilities, the exported workflow was deployed on a local Docker environment.

**1. Pull the n8n Docker Image:**
```bash
docker pull n8nio/n8n
```

**2. Start the n8n Container:**
```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```
> This runs n8n in the background, maps port `5678`, and creates a persistent Docker volume (`n8n_data`) to ensure workflows and credentials survive container restarts.

**3. Access the Local Instance:**
```
http://localhost:5678
```

**4. Import the Workflow:**
In the local n8n editor, use **Import from File** to load the [`Ice-Breaking Session.json`](Ice-Breaking%20Session.json) workflow.

**5. Reconnect Credentials:**
Since n8n does not export sensitive API keys for security, Gmail OAuth2 credentials must be manually reconnected to all three Gmail nodes on the local instance.

---

## 🐳 Docker Commands Cheat Sheet

| Action | Command |
|--------|---------|
| **Pull the image** | `docker pull n8nio/n8n` |
| **Create & start container** | `docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n` |
| **Stop the container** | `docker stop n8n` |
| **Start existing container** | `docker start n8n` |
| **View running containers** | `docker ps` |
| **View all containers** | `docker ps -a` |
| **View container logs** | `docker logs n8n` |
| **Remove the container** | `docker rm n8n` |

> **💡 Tip:** If you get a *"name is already in use"* error, the container already exists. Use `docker start n8n` instead of `docker run`.

---

## 📂 Repository Structure

```
Ice-Breaking-N8N/
├── README.md                        # This file
├── Ice-Breaking Session.json        # Exported n8n workflow (importable)
└── Results/
    ├── Cloud/
    │   └── Web.png                  # Screenshot: n8n Cloud workflow canvas
    ├── Docker/
    │   └── Image.png                # Screenshot: Self-hosted Docker workflow canvas
    ├── Test_Sender.png              # Screenshot: Sender's Gmail view
    └── Test_Reply.png               # Screenshot: Receiver's Gmail view (auto-reply)
```

---

## 💡 Key Learnings & Skills Demonstrated

| Skill | Description |
|-------|-------------|
| **Low-Code Automation** | Designed a node-based workflow using n8n's visual editor |
| **AI-Assisted Development** | Leveraged n8n's built-in AI Assistant for rapid prototyping |
| **API & OAuth2 Integration** | Authenticated and integrated Google Workspace (Gmail) APIs |
| **Cloud-to-Self-Hosted Migration** | Exported a cloud workflow and redeployed it in a local Docker environment |
| **Docker & Containerization** | Managed containerized services with persistent volumes |
| **Security Best Practices** | Handled credential re-linking post-migration; understood OAuth token management |
| **Version Control** | Maintained workflow JSON and documentation in a Git repository |

---

## 🧰 Tech Stack

<p align="center">
  <img src="https://img.shields.io/badge/n8n-FF6D5A?style=flat-square&logo=n8n&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Gmail_API-EA4335?style=flat-square&logo=gmail&logoColor=white" />
  <img src="https://img.shields.io/badge/OAuth_2.0-4285F4?style=flat-square&logo=google&logoColor=white" />
  <img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white" />
</p>

---

## 👤 Author

**Muhammad Khizar Naveed**  
🎓 BS Computer Engineering — Information Technology University (ITU), Lahore  
📧 [bsce22042@itu.edu.pk](mailto:bsce22042@itu.edu.pk)  
🔗 [GitHub — @KhizarNaveed](https://github.com/KhizarNaveed)

---

<p align="center">
  <i>⭐ If you found this project helpful, consider giving it a star!</i>
</p>
