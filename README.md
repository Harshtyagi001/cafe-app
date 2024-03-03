# CI/CD Pipeline to Deploy to Kubernetes Cluster using Jenkins
**This repo discusses about the full fledged Devops project build from scratch to practice some of the basic concepts of Jenkins, Ansible, Docker and Kubernetes.**

I am utilizing Jenkins to Deploy the Application to the Kubernetes Cluster using and using **Ansible** as a middleware to automate the process of fetching code from github and wrapping it to form an **Docker Image**,  pushing it to the **Dockerhub** and further to run the **Deployment** and **Service** manifests files on **Kubernetes** Server to finally serve my application to the end users.
The cloud provider I used for building this project is **Azure Cloud**.

## What's happening here?🤯🤯🤯
![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/4c166a7a-0941-47bd-abe7-bf4ccb9fa838)

## Don't worry! Let's have a project walkthrough😄

## Overview
This is project is fragmented into two parts i.e **CI Job (Continous Integration)** and **CD Job (Continous Deployment)**.
### Continous Integration
Whenever there is a new Commit on github repository, Jenkins pulls the code from Github and triggers the Ansible playbook which builds the Docker Image and push it to the Dockerhub.

### Continous Deployment
This process is triggered just after the successfull completion of CI Job and here it again runs the ansible playbook but this time for pulling Image from Dockerhub to further deploy the pods on Kubernetes Cluster.

## Detailed Walkthrough

### STEP-1: **Install and Configure Jenkins**
1. Create first Ubuntu Virtual machine named Jenkins at Microsoft Azure.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/21460e54-50b9-4d90-b8f2-cac2bda9b04b)

2. Installed java-sdk and then Jenkins by following official documentation and verify the status of jenkins up and running.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/cac5576d-7142-4179-a4ef-3fe7bb4223e2)
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/1141f3f3-6666-4ef4-8ff5-5d5a6f02de1c)
   
3. Log into Jenkins Console by typing public IP of Jenkins VM followed by port 8080 and download suggested plugins over there.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/aff13c79-9c30-4957-ab87-73c38ff9554d)

4. Create second Virtual Machine named Ansible.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/b5fce29b-8e32-4d71-8f0f-f0fdf4b86a18)

5. Install and configure Ansible on Ansible VM.

### STEP-2: **Integrate Ansible with Jenkins and Install Docker on Ansible Server.**
1. Download Plugin named Publish Over SSH and Add the Ansible Machine to allow Jenkins to communicate with it.
  ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/a8cb6bc0-cd0e-4ecc-bb70-7a685e04ba40)

2. Install Docker on Ansible VM and build Folder named Docker where I will add all the Playbooks needed for this project.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/f8623c73-2605-4d72-8f84-c7a2255da007)
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/e9e6c2df-b8f1-421b-810c-db5d2a6a52b7)

3. Add my default **ansibleuser** to the group **sudo** and **docker** to allow him to run all necessary commands of docker.
4. Add the group Ansible to the **Hosts** which tells to run the ansible playbook on defined host. Here I am using same Ansible Host to run the playbook needed for Continous Integration part.
5. Write a playbook for CI Job and run, then login to Dockerhub to validate if Image is pushed or not.
    ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/eef0ab00-b350-4857-8cc2-31126248349e)
    ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/c5424014-10aa-4cb5-ab3f-cdc6fec5d533)
    ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/2418d33f-3e03-41cd-8d7f-6d516711b36e)

### STEP-3: **Create Jenkins Job to Run Ansible Playbook.**
![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/b9125e02-1c24-4f46-9f41-5caf427d5f63)

### STEP-4: **Create Kubernetes Cluster.**
1. Create third Virtual Machine named **Kubeserver**.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/438ead35-98a7-4e0c-b48f-c31494b5ca79)
   
2. Create Kubernetes cluster named **waveapp-cluster**
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/87511666-f862-46e4-829e-400112cc34ad)

3. Connect to the Cluster
   ---- Login into azure portal by using command: az login --use-device-code , then download azure-cli and connect to your cluster by selecting command from Cluster Connect option.

### STEP-4: **Create Deployment and Service Manifest Files.**
**Deployment file** <br>
 ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/f4f2e03c-246b-4341-b849-2b352d8846a1) <br>
**Service File** <br>
 ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/47a50c90-da58-4212-9250-b0266f2b8f4c) <br>

### STEP-5: **Create Ansible Playbook for Deployment and Service Manifest Files.**
1. Create Ansible Playbook to trigger Deployment and Service Manifest Files.
  ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/1f005799-7639-4fc9-a2a9-d14728dc0728)

2. Display all the pods and services running.
3.  ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/b621a168-04c3-4d69-ab36-2da9667ca230)
4. Copy the External IP from above and type it on browser, you can see the Deployed Website on Cloud.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/d4c70a10-5382-4231-84b8-d479aa9f593c)

### STEP-6: Create Jenkins Deployment Job for Kubernetes
1. Create the freestyle project for CD Joband set the **exec** command as follows
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/c89815af-864f-4aac-87e9-c5130615a41c)

2. I want to trigger the CD Job only after successful buil of CI Job , so set such configuration showed below
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/f472b88f-9fec-4287-8a5d-c165cde95dc5)

## Now All seteup is done🤗!

## FINAL DEMO!!!

### Developer updated any piece of Code
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/a9bc8d9e-92d9-45aa-8602-395297ab4542)

### I have setted up the Jenkins in such a way that it looks at repostitory in every minute and triggers the CI Job if found any change in code.
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/1e88d404-b440-4c8f-a711-1bfe0e65dddc)

### After success of CI Job, It will automatically trigger the CD Job
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/61b64979-20f9-4ac3-ac89-f34c755c7959)
   ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/203ee9a6-e262-4cfe-bef8-c8d31c09448e)

### Booyah! Website gets updated.
  **BEFORE** <br> 
     ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/eca1ee71-009c-42c7-a4e2-1a53718694a1) <br>
  **AFTER**  <br>
     ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/ff7ddc19-84ba-4fc0-bc43-7c233b7a8a78)  <br>



