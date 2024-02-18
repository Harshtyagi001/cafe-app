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
5. Write a playbook for CI Job and login to Dockerhub to validate if Image is pushed or not.
    ![image](https://github.com/Harshtyagi001/cafe-app/assets/96621226/eef0ab00-b350-4857-8cc2-31126248349e)


   
