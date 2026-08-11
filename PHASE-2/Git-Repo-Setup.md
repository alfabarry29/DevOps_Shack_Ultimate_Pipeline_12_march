# Git Repository Setup for the DevOps Pipeline

This phase prepares the project for CI/CD automation by creating a GitHub repository, adding project files, and configuring Jenkins to pull source code from Git.

---

## 1. Create a GitHub Repository

1. Log in to GitHub.
2. Click on the New repository button.
3. Give the repository a name, for example:
   - devops-project
   - devops-application
4. Choose Public or Private.
5. Optionally initialize with:
   - README
   - .gitignore
   - license
6. Click Create repository.

---

## 2. Generate a GitHub Personal Access Token

If GitHub requires PAT-based authentication for CLI or HTTPS access:

1. Go to GitHub → Settings → Developer settings → Personal access tokens.
2. Generate a new classic or fine-grained token.
3. Select permissions such as:
   - repo
   - workflow
4. Copy the token and store it securely.

> Do not commit tokens into Git repositories.

---

## 3. Initialize the Project Locally

Open a terminal in your project folder:

```bash
git init
git add .
git commit -m "Initial commit"
```

If you are connecting to a newly created GitHub repo:

```bash
git branch -M main
git remote add origin <your-repository-url>
git push -u origin main
```

If you are cloning an existing repository:

```bash
git clone <repository-url>
cd <repository-folder>
```

---

## 4. Recommended Project Structure

Use a clean project layout so Jenkins and Kubernetes can work with it easily:

```bash
project/
├── app/
├── k8s/
├── Dockerfile
├── Jenkinsfile
├── .gitignore
├── README.md
├── requirements.txt
└── .github/
```

Example:

```bash
mkdir -p app k8s .github
touch Dockerfile Jenkinsfile .gitignore README.md
```

---

## 5. Add a Basic Dockerfile

Example for a Python application:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

---

## 6. Add a Basic Jenkinsfile

Example Jenkins pipeline:

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-user/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying the application..."'
            }
        }
    }
}
```

---

## 7. Push Code to GitHub

After creating your files, run:

```bash
git add .
git commit -m "Add application files and pipeline definition"
git push origin main
```

If GitHub asks for credentials:
- enter your GitHub username
- use the personal access token as the password

---

## 8. Configure Jenkins to Use GitHub

In Jenkins:

1. Go to Manage Jenkins → Manage Plugins.
2. Install:
   - Git Plugin
   - GitHub Plugin
3. Go to Manage Jenkins → Credentials.
4. Add GitHub credentials using:
   - username/password, or
   - personal access token
5. Create a new Pipeline job.
6. Set the repository URL.
7. Use the Jenkinsfile from your GitHub repo.

---

## 9. Trigger Builds Automatically

You can trigger Jenkins builds in two ways:

### Option 1: Polling SCM
Set Jenkins to check the repository periodically.

### Option 2: GitHub Webhooks
This is the better option for real CI/CD.

1. Open your GitHub repo.
2. Go to Settings → Webhooks.
3. Add a webhook.
4. Set the payload URL to your Jenkins webhook URL.
5. Choose push events.
6. Save the webhook.

---

## 10. Git Branching Best Practices

Use a clean workflow like this:

```bash
git checkout -b feature/login-page
git add .
git commit -m "Add login page"
git push origin feature/login-page
```

Then create a pull request and merge into main once approved.

---

## 11. Best Practices for the DevOps Pipeline

- Keep secrets out of Git
- Store credentials in Jenkins or GitHub secret stores
- Keep the Jenkinsfile in the repo root
- Use branches for features and fixes
- Merge only tested code
- Keep deployment manifests in the `k8s/` folder
- Use Docker for build consistency

---

## 12. Phase 2 Outcome

After completing this phase:
- the project exists in GitHub
- Jenkins can access the source code
- the repo is ready for build and deployment automation
- CI/CD setup is prepared for the next stage

This phase is the bridge between local development and automated pipeline execution.

---

## Summary

The essential steps for Phase 2 are:
1. Create a Git repository
2. Initialize project files
3. Commit and push code
4. Add a Jenkinsfile
5. Connect Jenkins to GitHub
6. Trigger builds automatically

This prepares the project for the next phase, where Jenkins will build, test, and deploy your application.
