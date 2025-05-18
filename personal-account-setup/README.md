# Project Setup Guide for Personal Accounts

This guide provides step-by-step instructions for setting up a production-ready project repository using your personal GitHub account.

## Step 1: Create the Project Repository

### 1.1. Create a New Repository

1. Log in to your GitHub account.
2. Click the **+** icon in the top-right corner and select **New repository**.
3. Provide the following details:

- **Repository Name**: Use a unique, descriptive, and meaningful name.
- **Description**: Add a concise summary of the project.
- **Visibility**: Choose **Private** for production-level projects to restrict access.

4. Initialize the repository with:

- A **README file**: Include a project overview and setup instructions.
- A **.gitignore file**: Select a template suitable for your tech stack (e.g., Node.js, Python).
- A **license**: Choose an appropriate license for your project.

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
3. Open a terminal and run:

```bash
git clone <repository-url>
```

Replace `<repository-url>` with the URL you copied.

#### Option 3: Push an Existing Repository

If you already have a local repository, connect it to GitHub using:

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

## Step 2: Configure Teams and Access Control

### 2.1. Add Collaborators

1. Go to **Repo** -> **Settings** -> **Collaborators and teams**.
2. Click **Add people**.
3. Search for users by username, full name, or email.
4. Add the user and assign the appropriate access level (e.g., Read, Write, Admin).

### 2.2. Use GitHub Teams (Optional)

For larger teams, create GitHub Teams to manage access:

1. Navigate to your organization -> **Teams**.
2. Create a new team and assign members.
3. Grant the team access to the repository with the desired permission level.

## Step 3: Set Up Project Management Tools

### 3.1. Create a GitHub Project (Kanban Board)

1. Go to **Projects**.
2. Click **New Project**.
3. Choose **Board** (Kanban-style) or **Table**.
4. Enter a **Project name** and click **Create Project**.

### 3.2. Define Workflow Columns

Customize columns to match your production workflow:

| **Column Name**   | **Description**                                                    |
| ----------------- | ------------------------------------------------------------------ |
| **Backlog**       | Ideas and suggestions not yet prioritized.                         |
| **To Do**         | Well-defined tasks ready for development.                          |
| **In Progress**   | Tasks actively being worked on.                                    |
| **Testing**       | Tasks undergoing testing (unit, integration, QA).                  |
| **In Review**     | Completed work awaiting peer review or stakeholder feedback.       |
| **To Deploy**     | Work that has passed review and testing, queued for deployment.    |
| **In Production** | Features or fixes deployed and live in the production environment. |
| **Blocked**       | Tasks stalled due to dependencies or issues.                       |
| **Done**          | Fully completed work requiring no further action.                  |

### 3.3. Automate Workflows

1. In your project, click the **Settings** gear icon.
2. Go to the **Workflows** tab and set rules like:

- "When issue is added → move to To Do".
- "When pull request is opened → move to In Review".
- "When issue is closed → move to Done".

## Step 4: Set Up GitHub Actions and Branch Protection Rules

### 4.1. Configure GitHub Actions

1. Create a workflow file:

```bash
mkdir -p .github/workflows
touch .github/workflows/ci.yml
```

2. Add a production-ready CI/CD configuration:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main
      - develop

jobs:
  build-and-test:
    name: Build and Test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "16"

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  deploy:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: build-and-test
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Deploy application
        run: echo "Deploying to production..."
```

3. Commit and push the workflow:

```bash
git add .github/workflows/ci.yml
git commit -m "Add CI/CD pipeline"
git push
```

### 4.2. Set Branch Protection Rules

1. Go to **Settings** -> **Branches** -> **Add branch ruleset**.
2. Configure the following rules:

- **Require a pull request before merging**.
- **Require status checks to pass before merging**.
- **Require branches to be up to date before merging**.
- **Require linear history**.
- **Restrict who can push to matching branches**.

3. Save the ruleset.

## Step 5: Start Development

### 5.1. Create a Feature Branch

```bash
git checkout -b feat/<feature-name>
```

Replace `<feature-name>` with a descriptive name for the feature.

### 5.2. Commit and Push Changes

```bash
git add .
git commit -m "feat: add <feature-description>"
git push origin feat/<feature-name>
```

### 5.3. Create a Pull Request

1. Navigate to your repository on GitHub.
2. Click **Compare & pull request**.
3. Provide a clear title and description.
4. Assign reviewers and click **Create pull request**.

### 5.4. Merge and Delete the Branch

1. After approval, merge the PR.
2. Delete the branch locally and remotely:

```bash
git branch -d feat/<feature-name>
git push origin --delete feat/<feature-name>
```
