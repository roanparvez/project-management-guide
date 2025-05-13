# Project Setup Guide for Personal Accounts

This guide provides step-by-step instructions for setting up a project repository using your personal GitHub account.

## Step 1: Create the Project Repository

### 1.1. Create a New Repository

1. Log in to your GitHub account.
2. Click the **+** icon in the top-right corner and select **New repository**.
3. Provide the following details:
   - **Repository Name**: Enter a unique and descriptive name.
   - **Description** (optional): Add a brief summary of the project.
   - **Visibility**: Choose between **Public** or **Private** based on your preference.
4. (Optional) Initialize the repository with:
   - A **README file**.
   - A **.gitignore file** (select a template suitable for your project).
   - A **license** (if applicable).
5. Click **Create repository** to complete the process.

### 1.2. Set Up the Repository Locally

#### Option 1: Initialize a New Repository Locally

Run the following commands in your terminal:

```bash
echo "# <repository-name>" >> README.md
git init
git add README.md
git commit -m "Initial commit"
git branch -M main
git remote add origin <repository-url>
git push -u origin main
```

Replace `<repository-name>` with your repository's name and `<repository-url>` with the URL of your GitHub repository.

#### Option 2: Clone the Repository

1. Navigate to your repository page on GitHub.
2. Click the **Code** button and copy the repository URL (HTTPS or SSH).
3. Open a terminal and run the following command:
   ```bash
   git clone <repository-url>
   ```
   Replace `<repository-url>` with the URL you copied.

#### Option 3: Push an Existing Repository

If you already have a local repository, connect it to GitHub using the following commands:

```bash
git remote add origin <repository-url>
git branch -M main
git push -u origin main
```

### Navigate to the Repository Directory

After cloning or initializing the repository, navigate to its directory:

```bash
cd <repository-name>
```

Replace `<repository-name>` with the name of your repository.

### Verify the Setup

To confirm the repository is correctly linked, run:

```bash
git remote -v
```

Ensure the output displays the correct repository URL.

## Step 2: Create Teams

### 2.1. Set Access for Individuals

- Go to **Repo** -> **Settings** -> **Collaborators**
- Click **Add people**
- Seach people by username, full name, or email
- Add the user and assign the appropriate access level

## Step 3: Set Up Project Management Tools

### 3.1. Create a GitHub Project (Kanban Board)

- Go to **Projects**
- Click **New Project**
- Choose **Board** (Kanban-style) or **Table**
- Write **Project name** and Click **Create Project** to create the project

### 3.2. Define Workflow Columns

- Customize columns to match your workflow:
  - Examples: To Do, In Progress, Testing, Review, To Deploy, In Production, Done.

### 3.3. Create & Link Issues to Your Project Board

#### Option 1: From the Project Board

- Go to your GitHub Project.
- In your Kanban board view, pick the column you want (e.g., To Do).
- Click **+ Add item** at the bottom of the column.
  - Type to search for an existing issue (#issue-number or title), or
  - Type # to select repository and Select a repository for you issue
  - Then Click **+ Create new issue**
  - In the popup:
    - Select the **Repository**
    - Click **Blank issue**
- If creating a new issue, a modal will open — fill in the title/description, and GitHub will:
  - Create the issue in your repo.
  - Link it automatically to your project.
  - Place it in the selected column.

#### Option 2: From an Issue

- Go to **Issues** and Click **New Issue**
- On the right sidebar, scroll to the **Projects** section.
- Select your Kanban project.
- Add a title, and decription
- The issue is now added to your project and visible in the default column (e.g., To Do).
- Then Click **Create**

### Optional: Set Up Authomation

- Want issues to automatically move based on status? Here's how:
  - In your project, click the **Settings** gear(... three dot) icon (top-right).
  - Go to the **Workflows** tab.
  - Set rules like:
    - "When issue is added → move to To Do"
    - "When pull request is opened → move to In Review"
    - "When issue is closed → move to Done"

## Step 4: Setup GitHub Actions and Branch Protection Rules

Setup GitHub Actions and Branch Protection Rules before working on the project

### 4.1 Set Up GitHub Actions

#### 4.1.1. Create GitHub Actions Workflow File

- In your project root, create:

```bash
.github/workflows/ci.yml
```

- Add a basic configuration:

Minimal `ci.yml` for f Fresh Repo

```
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  placeholder-check:
    name: Initial CI Scaffold
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup message
        run: echo "CI is set up. Add real steps once project code is added."
```

#### 4.1.2. Commit & Push Workflow

```bash
git add .github/workflows/ci.yml
git commit -m "Add GitHub Actions CI workflow"
git push
```

### 4.2. Configure Branch Protection Rules

- Navigate to the repository:
  - Go to **Repositories** -> Select your repository.
  - Click on **Settings** -> **Branches**.
  - Under the "Code and automation" section, click **Add branch ruleset**.

##### Ruleset Configuration

- **Ruleset Name**: Protect Main
- **Enforcement Status**: Active
- **Bypass List**: Leave empty.
- **Target Branches**:
  - Click **Add target** -> **Include by pattern**.
  - Enter branch patterns (e.g., `main`, `dev`).

##### Enable the Following Rules:

- ✅ **Require a pull request before merging**: Enforces code reviews and prevents direct commits.
- ✅ **Required approvals**: Minimum number: 1 (set to 2 for stricter reviews).
- ✅ **Dismiss stale pull request approvals when new commits are pushed**: Ensures reviews are up-to-date.
- ✅ **Require status checks to pass**: Add CI checks (e.g., build, test).
  - Ensure CI tools (e.g., GitHub Actions) are configured to list available checks.
- ✅ **Require branches to be up to date before merging**: Enforces rebasing or merging the latest `main` branch.
- ✅ **Require linear history**: Prevents merge commits, maintaining a clean commit history.
- ✅ **Restrict who can push to matching branches**: Limit to trusted teams (e.g., DevOps, Admin).
- ✅ **Do not allow bypassing the rules**: Prevents admins or bots from bypassing protections.

##### Optional:

- 🔒 **Include administrators**: Applies rules to organization/repository admins.

- 💾 Save the ruleset by clicking **Create** or **Save ruleset**.
