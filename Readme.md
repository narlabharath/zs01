# CI/CD Pipeline with GitHub Actions and Minikube

This guide walks you through setting up a CI/CD pipeline using GitHub Actions with a self-hosted runner on Windows. The goal is to automate the deployment of a containerized application to a local Minikube Kubernetes cluster using Docker.

## Workflow Overview

The CI/CD process involves the following main steps:

1. **Local Repository Setup:** Initialize a local Git repository and link it to a remote GitHub repository.
2. **GitHub Repository and Actions Setup:** Configure the GitHub repository and set up self-hosted GitHub Actions.
3. **Initial Push and Pipeline Trigger:** Push the initial commit to the repository, which triggers the pipeline.
4. **Make Code Changes and Re-Trigger Pipeline:** Push new changes to verify the CI/CD workflow.
5. **Service Verification:** Access the deployed frontend service through Minikube.
6. **Cleanup:** Delete all Kubernetes resources after testing.

---

## Step 1: Local Repository Setup

1. **Install Git:**

   * Download and install from [Git SCM](https://git-scm.com/downloads) using default settings.
2. **Initialize Repository:**

   ```bash
   cd \cicd_app
   git init
   ```
3. **Check Branch:**

   ```bash
   git branch
   ```

   * If on the default branch (usually `master`), switch to `main`:

   ```bash
   git checkout -b main
   ```

---

## Step 2: GitHub Repository and Actions Setup

1. **Create a New Repository on GitHub:** Use default settings.
2. **Self-Hosted Runner Configuration:**

   * Go to **Settings > Actions > Runners**.
   * Select **Self-Hosted Runners** and follow instructions for Windows.

   ```powershell
   ./config.cmd --url https://github.com/username/repo --token YOUR_TOKEN
   ./run.cmd
   ```
3. **Connect Remote Repository:**

   ```bash
   git remote add origin https://github.com/username/repo.git
   ```
4. **Update GROQ API Key:** Replace the key in the `main.py` file under the backend folder.

---

## Step 3: Initial Push and Pipeline Trigger

1. **Stage Changes:**

   ```bash
   git add .
   ```
2. **Commit Changes:**

   ```bash
   git commit -m "Initial commit"
   ```
3. **Configure Git Credentials:**

   ```bash
   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   ```
4. **Push to GitHub:**

   ```bash
   git push -u origin main
   ```

### What Happens After Step 3:

* The GitHub Actions pipeline is triggered immediately after the first push to the `main` branch.
* You can observe the progress in the **GitHub Actions** tab.

---

## Step 4: Make Code Changes and Re-Trigger Pipeline

1. **Make a Small Change to Code:**

   ```python
   # Triggering CI/CD pipeline again
   print("New deployment test")
   ```
2. **Push the Change:**

   ```bash
   git add .
   git commit -m "Triggering CI/CD again"
   git push
   ```
3. **Check Actions:**

   * The pipeline will be triggered again due to the push event.
   * Monitor the **GitHub Actions** tab.

---

## Step 5: Accessing the Deployed Service

1. **Check Pod Status:**

   ```bash
   kubectl get pods
   kubectl get svc
   ```
2. **Access Frontend Service:**

   ```bash
   minikube service frontend-service
   ```

---

## Step 6: Cleanup

To remove all resources:

```bash
kubectl delete deployment --all
kubectl delete service --all
minikube stop
```

---

## Troubleshooting

* **Minikube API Server Stopped:**

  ```bash
  minikube stop
  minikube start --driver=docker
  ```
* **GitHub Actions Not Triggering:** Check if the runner is running and correctly configured.
* **Deployment Errors:** Check the GitHub Actions logs for specific errors.

This guide ensures that you can efficiently set up, run, and maintain a CI/CD pipeline using GitHub Actions and Minikube. If you encounter any issues, refer to the troubleshooting section for guidance.
