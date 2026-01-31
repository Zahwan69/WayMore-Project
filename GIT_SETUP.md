# Git Setup Guide - Backend and Frontend Branches

Follow these steps to set up your GitHub repository with separate branches.

## Step 1: Initialize Git Repository

```bash
# Navigate to your project root
cd "C:\Users\zahuw\OneDrive\Desktop\WAYMORE PROJECT"

# Initialize git repository
git init

# Add all files
git add .

# Create initial commit on main branch
git commit -m "Initial commit: Project structure"
```

## Step 2: Create Backend and Frontend Branches (Locally)

```bash
# Create and switch to backend branch
git checkout -b backend

# Switch back to main
git checkout main

# Create and switch to frontend branch
git checkout -b frontend

# Switch back to main (we'll push from here)
git checkout main
```

## Step 3: Create GitHub Repository

1. Go to [GitHub.com](https://github.com) and sign in
2. Click the **+** icon in the top right → **New repository**
3. Name it: `waymore-project` (or your preferred name)
4. **Don't** initialize with README, .gitignore, or license (we already have files)
5. Click **Create repository**

## Step 4: Connect Local Repository to GitHub and Push All Branches

```bash
# Add GitHub remote (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/waymore-project.git

# Push main branch first
git checkout main
git push -u origin main

# Push backend branch
git checkout backend
git push -u origin backend

# Push frontend branch
git checkout frontend
git push -u origin frontend
```

## Step 5: Working with Branches

### Switch between branches:
```bash
git checkout backend    # Switch to backend branch
git checkout frontend   # Switch to frontend branch
git checkout main       # Switch to main branch
```

### Make changes and commit:
```bash
# After making changes
git add .
git commit -m "Your commit message"
git push
```

### Create new branch from existing:
```bash
git checkout -b new-feature-branch
```

## Branch Strategy

- **main**: Main production-ready code
- **backend**: All backend-related code and changes
- **frontend**: All frontend-related code and changes

## Tips

- Always commit and push your work before switching branches
- Use descriptive commit messages
- Pull latest changes before starting work: `git pull`

