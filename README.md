# CustomGHActions

GH 200 Exam Study Guide: 
* https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-200
* https://learn.microsoft.com/en-us/training/courses/gh-200t00

Morning:
* Automate development tasks by using GitHub Actions
* Build continuous integration (CI) workflows by using GitHub Actions
* Build and deploy applications to Azure by using GitHub Actions

Afternoon (Post Lunch)
* Leverage GitHub Actions to publish to GitHub Packages
* Create and publish custom GitHub actions
* Manage GitHub Actions in the enterprise

GH 200 Technical Videos: GH-200: Automate your workflow with GitHub Actions https://www.youtube.com/playlist?list=PLahhVEj9XNTd5N_seZDoRXVIn6N1qAp-_

GH-200: Automate your workflow with GitHub Actions https://docs.github.com/en/actions

GitHub Actions Docs: GitHub Actions documentation - GitHub Docs
https://docs.github.com/en/actions

Tutorials for GitHub Actions: https://docs.github.com/en/actions/tutorials
 
Microsoft Learn Live: Learn Live | Microsoft Learn
https://learn.microsoft.com/en-us/shows/learn-live/?products=github
 
Enhance your workflow with extensions: Github Actions with Azure: https://github.com/marketplace?query=Azure&type=actions
 
Create two deployment workflows using GitHub Actions and Microsoft: https://github.com/skills/deploy-to-azure

### Instructors
Dipan G: https://github.com/dipan-gandhi & Rajesh Kumar Gogia https://github.com/GH200OrgA


### GH workflows
https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax

https://learn.microsoft.com/en-us/training/modules/github-actions-automate-tasks/3-exercise-create-container-action
Exercise - Exercise - Create and run a basic GitHub Actions workflow - Training | Microsoft Learn
 
https://learn.microsoft.com/en-us/training/modules/github-actions-ci/3-exercise-ci-workflow-github
Exercise - Create the CI workflow on GitHub - Training | Microsoft Learn
 
### GH Packages
* Publishing a package https://docs.github.com/packages/learn-github-packages/publishing-a-package
https://github.com/skills/publish-docker-images
* Using GitHub Packages with your project's ecosystem https://docs.github.com/free-pro-team@latest/packages/using-github-packages-with-your-projects-ecosystem

* Pushing and pulling Docker images https://docs.github.com/packages/guides/pushing-and-pulling-docker-images

* https://github.com/skills/write-javascript-actions


### GHA deploy to AKS flow diagram (AI generated)

<img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/3c8300db-9d15-4732-ac5d-fe8781f846e4" />

Phase 1: Code and Image Preparation (GitHub Environment)
* Developer Pushes Code: The developer writes code and pushes it to the GitHub Repository.
* CI/CD Trigger: This code update automatically triggers the GitHub Actions CI/CD pipeline.
* Build and Push Image: GitHub Actions builds the application into a Docker container image and pushes it securely to the GitHub Container Registry (ghcr.io).
* Credential Setup: A Personal Access Token (PAT) with read:packages scope is generated in GitHub to allow external access to these private images.

Phase 2: Cluster Authentication (The Bridge)
* Secret Creation: The developer (or the CI/CD pipeline) runs a kubectl command to manually inject the GitHub PAT into the Azure Kubernetes Service (AKS) Cluster.
* Stored as Image Pull Secret: This credential lives inside your specific K8s Namespace as an encrypted Kubernetes Image Pull Secret.

Phase 3: Deployment and Scheduling (AKS Control Plane)
* Initiate Deployment: GitHub Actions (via automated workflow) or the developer (via CLI) triggers a deployment by applying the Application Deployment YAML to the cluster.
* YAML Authentication: The Deployment YAML references the pre-created Image Pull Secret to gain permission to access the private GitHub registry.
Pod Scheduling: The K8s Control Plane (Scheduler) validates the YAML request and assigns the new application Pods to an available AKS Node.

Phase 4: Runtime Execution (AKS Worker Nodes)
* Pulling the Image: The worker node's Container Runtime (such as containerd) communicates directly with ghcr.io. It presents the stored Image Pull Secret to authenticate.
* Running the App: Once authenticated, the node pulls down the private container image from GitHub Packages and launches your application
 
