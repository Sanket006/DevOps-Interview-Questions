# 📝 Contributing Guide — Interview_Questions

> A step-by-step reference for adding new content to this repository correctly and consistently.

---

## 📑 Table of Contents

- [Before You Add Anything](#-before-you-add-anything)
- [Folder Decision Guide](#-folder-decision-guide)
- [Adding a File to an Existing Folder](#-adding-a-file-to-an-existing-folder)
- [Creating a New Topic Folder](#-creating-a-new-topic-folder)
- [File Naming Rules](#-file-naming-rules)
- [File Templates](#-file-templates)
  - [Core Q&A File](#1-core-qa-file-template)
  - [Real-World Troubleshooting File](#2-real-world-troubleshooting-file-template)
  - [Production Scenario File](#3-production-scenario-file-template)
  - [Company-Specific File](#4-company-specific-interview-file-template)
  - [HR Interview File](#5-hr-interview-file-template)
- [After Creating a File](#-after-creating-a-file-update-the-readmes)
- [Full Workflow Checklist](#-full-workflow-checklist)
- [What NOT to Do](#-what-not-to-do)

---

## 🔍 Before You Add Anything

Ask yourself two questions:

1. **Does a similar file already exist?**
   - Search the folder first: `Ctrl + Shift + F` in VS Code, or `grep -r "your topic" Interview_Questions/`
   - If yes — **add to the existing file** rather than creating a new one

2. **Which folder does it belong in?**
   - Use the [Folder Decision Guide](#-folder-decision-guide) below

---

## 📂 Folder Decision Guide

| Your content type | Correct folder |
|---|---|
| HR, self-introduction, behavioral, salary negotiation | [`Hr_Interview_QA/`](./Hr_Interview_QA/) |
| Tool-specific Q&A (one technology per file) | [`Core_Topic_Q&A/`](./Core_Topic_Q&A/) |
| Role-type or company-style interview rounds | [`Interview_Q&A/`](./Interview_Q&A/) |
| Quick-fire, speed-round revision questions | [`Rapid_fire_Mock_Interview_Q&A/`](./Rapid_fire_Mock_Interview_Q&A/) |
| Senior-level, cross-domain, multi-tool scenarios | [`High_Level_Mixed_Question_sets/`](./High_Level_Mixed_Question_sets/) |
| Production failure simulations (structured scenario → fix) | [`Production_Style_Question_Sets/`](./Production_Style_Question_Sets/) |
| Real incident-based debugging and troubleshooting | [`Real_World_Scenario_Based_Troubleshooting_Q&A/`](./Real_World_Scenario_Based_Troubleshooting_Q&A/) |
| A specific company's interview prep | [`Company_Wise_Interview_Q&A/`](./Company_Wise_Interview_Q&A/) |
| Questions based on your resume or a specific project | [`Resume_&_Project_based_DevOps_Interview_Q&A/`](./Resume_&_Project_based_DevOps_Interview_Q&A/) |
| PDF reference materials, question banks | [`PDF_for_Learning_Purpose/`](./PDF_for_Learning_Purpose/) |

### 🤔 Still Unsure? Use This Flowchart

```
Is it HR / behavioral?
    YES → Hr_Interview_QA/
    NO  ↓

Is it about ONE specific tool (Docker, K8s, Terraform...)?
    YES → Core_Topic_Q&A/
    NO  ↓

Is it a real incident / debugging story?
    YES → Real_World_Scenario_Based_Troubleshooting_Q&A/
    NO  ↓

Is it a simulated production failure scenario?
    YES → Production_Style_Question_Sets/
    NO  ↓

Is it for a specific company?
    YES → Company_Wise_Interview_Q&A/
    NO  ↓

Is it based on your resume or project?
    YES → Resume_&_Project_based_DevOps_Interview_Q&A/
    NO  ↓

Is it advanced / senior-level / multi-domain?
    YES → High_Level_Mixed_Question_sets/
    NO  → Interview_Q&A/
```

---

## ➕ Adding a File to an Existing Folder

Use this when your content **fits an already-existing folder**.

### Step 1 — Pick the right folder
Use the [Folder Decision Guide](#-folder-decision-guide) above.

### Step 2 — Name your file
Follow the [File Naming Rules](#-file-naming-rules) below.

### Step 3 — Create the file using the right template
Use one of the [File Templates](#-file-templates) further below.

### Step 4 — Update the sub-folder `README.md`
Open the `README.md` inside the folder you added to and add **one row** to its file table:

```markdown
| `your-new-file.md` | One-line description of what it covers |
```

### Step 5 — Update the main `README.md` *(if it's a significant file)*
Open [`Interview_Questions/README.md`](./README.md) and add the file to the matching section table.

---

## 🗂️ Creating a New Topic Folder

Use this when your content **does not fit any existing folder** — for example, a whole new interview category, a new company block, or a new specialisation.

### When Should You Create a New Folder?

> [!IMPORTANT]
> Only create a new folder when you have **at least 3 files** to put in it. A folder with 1 file adds clutter without benefit — add that file to the closest existing folder instead.

| Situation | Action |
|---|---|
| New company with 3+ interview files | New folder inside `Company_Wise_Interview_Q&A/` |
| New specialisation with 3+ Q&A files (e.g., DevSecOps, Platform Eng.) | New top-level topic folder |
| New resume or project variant | New sub-folder inside `Resume_&_Project_based_DevOps_Interview_Q&A/Resume/` |
| Single new file on any topic | Add to the most relevant **existing** folder |

---

### Step-by-Step: Create a New Topic Folder

#### Step 1 — Name the folder

Follow the `Title_Case_with_underscores` convention used by all existing folders:

```
✅ Good folder names:
   DevSecOps_Interview_Q&A/
   Platform_Engineering_Q&A/
   SRE_Interview_Q&A/
   Cloud_Native_Interview_Q&A/

❌ Avoid:
   my new folder/          → spaces
   devops stuff/           → lowercase + vague
   New_Folder_1/           → non-descriptive
```

#### Step 2 — Create the folder and add your files

Create the folder, then add at least 2–3 content files following the [File Naming Rules](#-file-naming-rules) and [File Templates](#-file-templates) below.

**Minimum recommended structure for a new folder:**

```
YourNewFolder/
├── README.md              ← Required — see template below
├── file-one.md            ← At least 2 content files
└── file-two.md
```

---

#### Step 3 — Create the folder `README.md`

Every folder **must** have a `README.md`. Copy and fill in this template:

```markdown
# [Emoji] [Folder Title]

> One-sentence description of what this folder contains and who it is for.

---

## 📌 Purpose

2–3 sentences explaining:
- What type of content is in this folder
- Who should use it
- When to use it (which phase of preparation)

---

## 📂 Files in This Folder

| File | Topics Covered |
|---|---|
| `file-one.md` | Brief description |
| `file-two.md` | Brief description |

---

## 💡 How to Use

1. Step one — e.g., "Start with file-one.md for fundamentals"
2. Step two — e.g., "Move to file-two.md for advanced scenarios"
3. Step three — e.g., "Practice answers out loud before the interview"

---

## ➡️ What to Study Next

After this folder, move to:
→ [`NextFolder/`](../NextFolder/)

---

*Part of the [Interview_Questions](../README.md) preparation hub.*
```

---

#### Step 4 — Register the folder in the main `README.md`

Open [`Interview_Questions/README.md`](./README.md) and make **three updates**:

**A. Add to the Table of Contents:**
```markdown
- [Your New Folder Title](#-your-new-folder-title)
```

**B. Add to the Repository Structure tree:**
```
├── YourNewFolder/                    # One-line description of folder
│   ├── file-one.md
│   └── file-two.md
```

**C. Add a full section in Contents Overview:**
```markdown
### [Emoji] Your New Folder Title

**Location:** `YourNewFolder/`

Brief description of what this folder covers.

| File | Topics Covered |
|---|---|
| `file-one.md` | Description |
| `file-two.md` | Description |
```

---

#### Step 5 — Add to the study strategy *(if applicable)*

If your new folder fits into the 4-phase study plan, mention it in the relevant phase inside the main README.

---

### ✅ New Folder Checklist

```
[ ] Folder has a clear, descriptive name (Title_Case_with_underscores)
[ ] Folder contains at least 2–3 files (not just 1)
[ ] README.md created inside the new folder
[ ] All files follow lowercase-with-hyphens naming
[ ] All files use the correct template
[ ] Main README.md — TOC updated
[ ] Main README.md — Repository Structure tree updated
[ ] Main README.md — Contents Overview section added
[ ] Folder does not duplicate an existing folder's purpose
```

---

## 📛 File Naming Rules

### ✅ The Rule: `lowercase-words-separated-by-hyphens.md`

| Folder | Naming Pattern | Example |
|---|---|---|
| `Core_Topic_Q&A/` | `q&a-[technology].md` | `q&a-azure-devops.md` |
| `Interview_Q&A/` | `interview-q&a-[company/role]-[topic].md` | `interview-q&a-google-sre.md` |
| `Production_Style_Question_Sets/` | `production-style-[tool]-scenario-based-q&a.md` | `production-style-azure-scenario-based-q&a.md` |
| `Real_World_Scenario_Based_Troubleshooting_Q&A/` | `real-world-scenario-based-[tool]-troubleshooting.md` | `real-world-scenario-based-azure-troubleshooting.md` |
| `High_Level_Mixed_Question_sets/` | `advanced-q&a-[topic].md` | `advanced-q&a-azure-aws-devops.md` |
| `Hr_Interview_QA/` | `hr-interview-q&a-[topic].md` | `hr-interview-q&a-remote-work.md` |
| `Company_Wise_Interview_Q&A/` | `[company-name]-devops-interview-readme.md` | `wipro-devops-interview-readme.md` |

### ❌ What to Avoid

```
❌ My New File.md              → spaces in filename
❌ MyNewFile.md                → PascalCase
❌ NEW_TOPIC.MD                → uppercase
❌ notes.md                    → too vague
❌ temp-file.md                → non-descriptive
❌ Q&A Docker.md               → mixed case + space
```

---

## 📄 File Templates

Copy the appropriate template below when creating a new file.

---

### 1. Core Q&A File Template

> Use for: `Core_Topic_Q&A/`

```markdown
# 🔧 [Technology] — Interview Q&A

> Focused Q&A covering [Technology] from beginner to advanced level.

---

## Q1. [Question here]?

**Answer:**

**Definition:** What it is in one sentence.

**How it works:** Technical explanation in 2–4 sentences.

**Example:**
```bash
# Code, command, or config example
example-command --flag value
```

---

## Q2. [Next question]?

**Answer:**

...

---

## Q3. [Question]?

**Answer:**

...
```

---

### 2. Real-World Troubleshooting File Template

> Use for: `Real_World_Scenario_Based_Troubleshooting_Q&A/`

```markdown
# 🔍 Real-World [Tool] Troubleshooting Q&A

> Incident-based debugging scenarios for [Tool] — based on real production problems.

---

## Scenario 1: [Brief problem title]

**Situation:** Describe what happened. What was the alert, symptom, or failure?

**Investigation:**

Step 1 — First thing you check:
```bash
command-to-run --here
```

Step 2 — What the output tells you:
```
example output showing the problem
```

Step 3 — Root cause identified:
> Explain what the root cause was.

**Fix:**
```bash
fix-command --applied
```

**Prevention:** What you put in place so this doesn't happen again.

---

## Scenario 2: [Next problem]

...
```

---

### 3. Production Scenario File Template

> Use for: `Production_Style_Question_Sets/`

```markdown
# 🏭 Production-Style [Tool] Scenario Q&A

> Structured production failure scenarios for [Tool]. Read the problem, attempt your own solution, then check the answer.

---

## Scenario 1: [Problem Title]

**🔴 Problem Statement:**
> [Describe the production problem as it would appear to an on-call engineer]

**Your tasks:**
1. What is your first action?
2. What commands do you run?
3. What is the root cause?
4. How do you fix it?
5. How do you prevent it?

---

**✅ Reference Answer:**

**Step 1 — Immediate triage:**
```bash
kubectl get pods -n <namespace>
kubectl describe pod <pod-name>
```

**Step 2 — Investigate:**
```bash
kubectl logs <pod-name> --previous
```

**Root cause:** Explanation of what caused the failure.

**Fix:**
```bash
fix-command
```

**Prevention:** Policy, alert, or process to prevent recurrence.

---

## Scenario 2: [Next problem]

...
```

---

### 4. Company-Specific Interview File Template

> Use for: `Company_Wise_Interview_Q&A/`

```markdown
# 🏢 [Company Name] — DevOps Interview Preparation

> Mock interview guide tailored to [Company Name]'s DevOps hiring process.

![Company](https://img.shields.io/badge/Company-[Name]-blue?style=flat-square)
![Role](https://img.shields.io/badge/Role-DevOps%20Engineer-green?style=flat-square)

---

## 📌 About the Role

- **Company:** [Company Name]
- **Role:** DevOps / Cloud Engineer
- **Stack:** [Key technologies they use]
- **Interview Rounds:** [Number of rounds and format]

---

## 🔍 Interview Format

| Round | Type | Duration | Focus |
|---|---|---|---|
| Round 1 | HR / Screening | 30 min | Background, motivation |
| Round 2 | Technical | 60 min | Core DevOps tools |
| Round 3 | System Design | 45 min | Architecture & scenarios |

---

## 📋 Round 1 — HR Questions

**Q: Tell me about yourself.**

**A:** [Structured answer]

---

## 💻 Round 2 — Technical Questions

**Q: [Technical question specific to this company's stack]**

**A:** [Answer]

---

## 🏗️ Round 3 — Scenario / System Design

**Q: [Scenario question]**

**A:** [Structured answer with architecture]

---

## 💡 Tips for This Company

- [Specific tip 1]
- [Specific tip 2]
- [Specific tip 3]
```

---

### 5. HR Interview File Template

> Use for: `Hr_Interview_QA/`

```markdown
# 👥 HR Interview Q&A — [Topic / Section Title]

> Behavioral and HR questions focused on [topic].

---

## Q1. [HR question]?

**Answer:**

**Opening:** [1–2 sentence opener]

**Body:** [Main answer — use STAR format if it's a behavioral question]
- **Situation:** ...
- **Task:** ...
- **Action:** ...
- **Result:** ...

**Closing:** [Brief confident closing statement]

---

## Q2. [Next question]?

**Answer:**

...
```

---

## ✅ After Creating a File — Update the READMEs

This is the step most people forget. Every folder has a `README.md` file table. **You must update it** whenever you add a new file.

### Step 1 — Open the sub-folder README

Navigate to the folder you added your file to and open its `README.md`.

### Step 2 — Add a row to the file table

Find the relevant table and add one row:

```markdown
| `your-new-file.md` | Brief description of what it covers |
```

**Example** — adding to `Core_Topic_Q&A/README.md`:

```diff
 | `q&a-advance-devops.md` | Advanced DevOps — GitOps, SRE, platform engineering |
+| `q&a-azure-devops.md`   | Azure DevOps — Pipelines, Boards, Repos, Artifacts  |
```

### Step 3 — Update the main README (for significant additions)

If you added a major new file or created a new company entry, also update [`Interview_Questions/README.md`](./README.md) in the matching section's table.

---

## 🔁 Full Workflow Checklist

Use this every time you add something new:

```
[ ] 1. Searched existing files — no duplicate found
[ ] 2. Identified the correct folder using the decision guide
[ ] 3. Named the file using lowercase-with-hyphens convention
[ ] 4. Used the correct template for the file type
[ ] 5. Updated the sub-folder README.md — added one row to the file table
[ ] 6. (If major) Updated the main Interview_Questions/README.md too
[ ] 7. No sensitive data included (passwords, tokens, personal details)
```

---

## 🚫 What NOT to Do

| ❌ Don't | ✅ Do Instead |
|---|---|
| Drop files at the root `Interview_Questions/` level | Put them in the correct sub-folder |
| Use vague filenames like `notes.md` or `temp.md` | Use descriptive, consistent names |
| Create a new folder for one file | Add to an existing folder or discuss first |
| Skip updating the README | Always update the sub-folder README |
| Include API keys, passwords, or tokens | Use placeholder text like `<YOUR_TOKEN>` |
| Duplicate content that already exists | Extend the existing file instead |
| Use Title Case or spaces in filenames | Use `lowercase-with-hyphens` only |

---

## 🔗 Quick Links

| Resource | Link |
|---|---|
| Main README | [Interview_Questions/README.md](./README.md) |
| HR folder | [Hr_Interview_QA/](./Hr_Interview_QA/) |
| Core Topic Q&A | [Core_Topic_Q&A/](./Core_Topic_Q&A/) |
| Real-World Troubleshooting | [Real_World_Scenario_Based_Troubleshooting_Q&A/](./Real_World_Scenario_Based_Troubleshooting_Q&A/) |
| Production Scenarios | [Production_Style_Question_Sets/](./Production_Style_Question_Sets/) |
| Company-Wise | [Company_Wise_Interview_Q&A/](./Company_Wise_Interview_Q&A/) |
| PDF Resources | [PDF_for_Learning_Purpose/](./PDF_for_Learning_Purpose/) |

---

<div align="center">
  <sub>Follow this guide to keep the repository clean, consistent, and useful for everyone.</sub>
</div>
