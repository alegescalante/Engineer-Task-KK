# Day 2: Create an Azure Virtual Machine

The Nautilus DevOps team is planning to migrate a portion of their infrastructure to the Azure cloud incrementally. As part of this migration, you are tasked with creating an Azure Virtual Machine (VM).

The requirements are:

1) Use the existing resource group.
2) The VM name must be **`devops-vm`**, it should be in **`West US`** region.
3) Use the **`Ubuntu 22.04 LTS`** image for the VM.
4) The VM size must be **Standard_B1s**.
5) Attach a default Network Security Group (NSG) that allows inbound **`SSH (port 22)`**.
6) Attach a **`30 GB`** storage disk of type **`Standard HDD`**.
7) The rest of the configurations should remain as default.

Step 1: Go to Virtual Machine Dashboard and CLick on `Create` to select `Virtual Machine`

<img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/281755ef-31e8-4406-8319-3c5608d7edc4" />

Step 2: Select Default resource group, vm name and region.

<img width="983" height="365" alt="image" src="https://github.com/user-attachments/assets/7a35ed05-1882-4a36-9e94-b41ad1d0f4e2" />

Step 3: Select Image and Size

<img width="1086" height="365" alt="image" src="https://github.com/user-attachments/assets/0ae80d66-c8d3-41a8-aadc-ccb6ffaaafcb" />

Step 4: Create SSH Key based on RSA(Optional) and Allow inbound traffic for SSH on port 22.

<img width="1156" height="380" alt="image" src="https://github.com/user-attachments/assets/159eb451-4cc8-4bff-a81e-b49229ebaea5" />

Step 5: Click on Disk and Select Disk size and Disk type. (Select Standard HDD for this task)

<img width="1235" height="381" alt="image" src="https://github.com/user-attachments/assets/139228d8-7d2f-45f5-8689-0c1f5bde30dd" />

Step 6: Review and Click on `Create`

<img width="1236" height="405" alt="image" src="https://github.com/user-attachments/assets/8a016ef8-44c9-44e8-b78e-c76fbab80b13" />