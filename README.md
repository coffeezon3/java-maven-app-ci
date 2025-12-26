# Java Maven App CI/CD Project

This repository documents the progress of the CI/CD project for a Java Maven application using Jenkins, Docker, and Git.

## Project Overview

The goal of this project is to create a **CI pipeline for a Java Maven application** that automates the following steps:

1. Clone the repository  
2. Build the JAR using Maven  
3. Build a Docker image  
4. Push the Docker image to a private repository  

---

## 1. Completed Project Tasks

### a) Install Tools in Jenkins

- Maven and NodeJS were installed in Jenkins under **Manage Jenkins → Global Tool Configuration**.  
- Proof: Jenkins configuration shows the tools installed.

### b) Make Docker Available on Jenkins Server

- Docker was installed on the Jenkins server and is accessible from Jenkins jobs.  
- Proof: Verified by running `docker --version` in a Jenkins job.

### c) Create Jenkins Credentials for Git Repository

- Credentials for GitHub were created under **Manage Jenkins → Credentials**.  
- Proof: Jenkins configuration shows the credentials exist.

### d) Create Different Job Types

- Freestyle Job, Pipeline Job, and Multibranch Pipeline were created for the Java Maven project.  
- Proof: Jenkins configuration shows the different job types.

### e) Clone Repository & Build Maven JAR Successfully

- The Git repository was cloned from GitHub.  
- Maven build successfully created the JAR file.  
- Proof: Jenkins console output shows successful build and JAR generation.

---

## 2. Proof of Git Repository Connection

- Jenkins successfully checked out the repository.  
- Proof Jenkins: Console output during the build shows repository cloning.  
- Proof locally:

```bash
git remote -v
git log --oneline -5
