# CICD-Project

# **CI/CD Pipeline for a Java Application using Jenkins, Docker, SonarQube, and ArgoCD**

## **Overview**
This project demonstrates a **CI/CD pipeline** setup for a **Java-based application** using various DevOps tools. It covers **Continuous Integration (CI)** with Jenkins, **Static Code Analysis** using SonarQube, **Containerization** with Docker, and **Continuous Delivery (CD)** using Kubernetes with ArgoCD.

![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)


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

![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.05.48_1c4dca0b.jpg)


3. **Jenkins Installation & Setup**
   - Installed **OpenJDK** (Jenkins requires Java).  
   - Installed **Jenkins** and logged in using port `8080`.  
   - Created a **new pipeline** specifying the **source code repo & Jenkinsfile path**.  

![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.06.11_68008552.jpg)


4. **Docker as Jenkins Agent**
   - Instead of using separate EC2 slaves, used **Docker as an agent** in the Jenkinsfile.  

5. **Maven Build**
   - Since the application is written in Java, used **Maven** to compile and package it.  

6. **SonarQube Setup & Integration**
   - Created a new **SonarQube user** on the same EC2 instance and installed SonarQube.  
   - Configured **SonarQube Scanner plugin** in Jenkins.  
   - Integrated **SonarQube credentials** into Jenkins.  

![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.09.31_912e93fd.jpg)


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

![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.29.03_699688c1.jpg)
![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.36.08_fcebd8d3.jpg)
![Alt Text](https://github.com/teja-105/CICD-Project/blob/c1204659dfe8ac7d38830ce1d8ead992d43b3702/WhatsApp%20Image%202025-02-19%20at%2021.39.58_ffcc8737.jpg)

---

## **How the Pipeline Works?**
```mermaid
graph TD;
    A[Developer] -->|Push Code| B[GitHub];
    B -->|Trigger Build| C[Jenkins];
    C -->|Build & Test| D[Maven];
    D -->|Analyze Code| E[SonarQube];
    E -->|Feedback| C;
    C -->|Build Image| F[Docker];
    F -->|Push Image| G[DockerHub];
    C -->|Update Deployment YAML| H[GitHub Deployment Repo];
    H -->|ArgoCD Detects Change| I[ArgoCD];
    I -->|Deploy Application| J[Kubernetes Cluster];

