
# Task
Create an SSH key pair in Azure with the following:
* Name: devops-kp
* Key type: RSA

Azure allows SSH key creation through the portal interface and also through the command line using Azure CLI. This article explains both methods with short examples.

# 1. What is an SSH Key Pair
SSH keys provide secure access to servers without using passwords.
An SSH key pair contains:
* A private key stored securely on your workstation
* A public key that you share with the server or cloud service

Azure commonly uses SSH keys for Linux virtual machines. RSA is one of the supported cryptographic algorithms.

# 2. Creating an SSH Key Pair using Azure Portal (UI Method)
Below is the simple and visual approach for beginners.

## Step 1: Open Azure Portal
Sign in at: https://portal.azure.com

## Step 2: Search for “SSH keys”
Use the global search bar and select SSH keys from the services list.

<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/68040ccd-0f94-4474-9248-08363e0eedd5" />


Azure Home Page
## Step 3: Create a New SSH Key
Select **Create**.

Fill out the fields:
* Resource Group: Select or create a new one
* Name: devops-kp
* Region: Choose the nearest region
* Key type: Select RSA
* Key size: Keep the default (2048 or 4096)

<img width="720" height="394" alt="image" src="https://github.com/user-attachments/assets/1f8b8fad-7c16-489d-b003-fb92851e126b" />


SSH Key
## Step 4: Download the Key
Azure creates the key and exposes the public key.
Download the private key to your local system.


You now have an RSA key pair named devops-kp stored under your Azure subscription.

## 3. Creating an SSH Key Pair using Azure CLI (Terminal Method)
This method is useful when you work with automation or remote servers.
Before starting, log in to Azure:
```bash
az login
```
Once authentication is complete, create the key pair with the required attributes.

## Create RSA SSH Key Pair
```bash
az sshkey create \
  --name devops-kp \
  --resource-group <your-resource-group> \
  --location <azure-region> \
  --public-key-file ~/.ssh/devops-kp.pub
 ```
Azure automatically saves and registers the public key.
The private key stays in your .ssh directory.

## Generate RSA Key Locally (alternate method)
If the task requires generating keys entirely on the client host:
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/devops-kp
```
Then upload the public key manually to Azure:
```bash
az sshkey create \
  --name devops-kp \
  --resource-group <your-resource-group> \
  --public-key-file ~/.ssh/devops-kp.pub
 ```
## 4. Verifying the Key
To list all SSH keys stored in your Azure subscription:
```bash
az sshkey list --output table
 ```
You should see devops-kp listed.

## 5. Practical Example of Usage
Once the SSH key is ready, use it when creating a Linux virtual machine.
```bash
az vm create \
  --name demo-vm \
  --resource-group <your-resource-group> \
  --image UbuntuLTS \
  --admin-username azureuser \
  --ssh-key-name devops-kp
 ```
The VM will allow secure access only through this key.

## Conclusion
Day 1 of this journey helps build a foundational security practice. SSH keys are used across virtual machines, Kubernetes clusters, container instances, and automation pipelines. Knowing both UI and CLI methods prepares you for real projects and exam scenarios.
