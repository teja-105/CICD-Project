# CICD-Project

# **CI/CD Pipeline for a Java Application using Jenkins, Docker, SonarQube, and ArgoCD**

## **Overview**
This project demonstrates a **CI/CD pipeline** setup for a **Java-based application** using various DevOps tools. It covers **Continuous Integration (CI)** with Jenkins, **Static Code Analysis** using SonarQube, **Containerization** with Docker, and **Continuous Delivery (CD)** using Kubernetes with ArgoCD.

The inspiration for this project comes from the **Jenkins-Zero-to-Hero** repository by [@abhishek-veeramalla](https://github.com/abhishek-veeramalla). A huge thanks to him for providing such an amazing learning resource! 🚀

---

## **Tech Stack & Tools Used**
- **Jenkins** – For automating the CI/CD pipeline  
- **Docker** – For containerization of the Java application  
- **Maven** – For building the Java application  
- **SonarQube** – For static code analysis  
- **Minikube** – As a local Kubernetes cluster  
- **ArgoCD** – For GitOps-based Continuous Delivery  
- **Kubernetes Operators** – To manage ArgoCD lifecycle  
- **GitHub** – As the source code repository  

---

## **CI/CD Pipeline Workflow**

### **Continuous Integration (CI) Process**
1. **Source Code Management**  
   - The Java application is stored in a GitHub repository.  

2. **EC2 Instance Setup**  
   - Launched a **t2.large EC2 instance** to install required tools.  

3. **Jenkins Installation & Setup**
   - Installed **OpenJDK** (Jenkins requires Java).  
   - Installed **Jenkins** and logged in using port `8080`.  
   - Created a **new pipeline** specifying the **source code repo & Jenkinsfile path**.  

4. **Docker as Jenkins Agent**
   - Instead of using separate EC2 slaves, used **Docker as an agent** in the Jenkinsfile.  

5. **Maven Build**
   - Since the application is written in Java, used **Maven** to compile and package it.  

6. **SonarQube Setup & Integration**
   - Created a new **SonarQube user** on the same EC2 instance and installed SonarQube.  
   - Configured **SonarQube Scanner plugin** in Jenkins.  
   - Integrated **SonarQube credentials** into Jenkins.  

7. **Restart Jenkins** to apply all configurations.  

---

### **Continuous Delivery (CD) Process**
1. **Minikube Setup**  
   - Installed **Minikube** as a local Kubernetes cluster for deployment.  

2. **Kubernetes Controllers Setup**  
   - Installed **kubectl** for cluster management.  
   - Used **Kubernetes Operators** to manage the lifecycle of controllers.  

3. **ArgoCD Installation & Configuration**  
   - Installed **ArgoCD Controller** from **OperatorHub.io**.  
   - Modified `example-argocd-server` from **ClusterIP** to **NodePort** for public access.  
   - Logged into **ArgoCD UI** and integrated CI/CD using a **Shell Script**.  

4. **Deployment Process**  
   - Docker image is **built & pushed** to a container registry.  
   - The deployment YAML file is **updated** (typically in a separate GitOps repository, but in this case, stored in the same repo).  
   - **ArgoCD detects the changes** and deploys the latest version to **Minikube Kubernetes Cluster**.  

---

## **How the Pipeline Works?**
```mermaid
graph TD;
    Developer--Push Code-->GitHub;
    GitHub--Trigger Jenkins-->Jenkins;
    Jenkins--Build & Test-->Maven;
    Maven--Analyze Code-->SonarQube;
    SonarQube--Feedback-->Jenkins;
    Jenkins--Build Image-->Docker;
    Docker--Push Image-->DockerHub;
    Jenkins--Update Deployment YAML-->GitHub;
    GitHub--ArgoCD Detects Change-->ArgoCD;
    ArgoCD--Deploy to K8s-->Kubernetes Cluster;
