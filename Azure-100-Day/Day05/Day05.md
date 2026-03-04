# Day 5: Create a Virtual Network (IPv4) in Azure

Create a **`Virtual Network`** (VNet) named **`nautilus-vnet`** in the **`East US`** region with **`192.168.0.0/24`** IPv4 CIDR.

The following procedure creates a virtual network with a resource subnet, an Azure Bastion subnet, and a Bastion host:

- In the portal, search for and select **`Virtual networks`**.
- On the **`Virtual networks`** page, select + **`Create`**.
- On the **`Basics`** tab of **`Create virtual network`**, enter, or select the following information:

  <img width="1193" height="475" alt="image" src="https://github.com/user-attachments/assets/550d241f-212b-47bc-882f-ead5f9b3d91a" />

- Select **`Next`** to proceed to the **`Security`** tab.
- Select **`Next`** to proceed to the **`IP Addresses`** tab and Delete all address spaces.

  <img width="1194" height="483" alt="image" src="https://github.com/user-attachments/assets/4ce67e19-fba8-4e9e-a859-7b2b74c4daf3" />

- In **`Add IPv4 Address space`**, enter **`192.168.0.0/24`** Address Space for challenge requirment.

  <img width="1198" height="483" alt="image" src="https://github.com/user-attachments/assets/5aa108ee-a861-4a5a-b125-86b03896b0ed" />

- Select **`Save`**.
- Select **`Review + create`** at the bottom of the window. When validation passes, select **`Create`**.

  <img width="420" height="60" alt="image" src="https://github.com/user-attachments/assets/3c7f47e2-aa98-42b2-8669-b11165aa721a" />

- Verify Deployment

  <img width="776" height="302" alt="image" src="https://github.com/user-attachments/assets/3366d675-dd2f-4598-93b8-6f48992ef35b" />


**Azure Docs to be followed: [Create an Azure Virtual Network](https://learn.microsoft.com/en-us/azure/virtual-network/quickstart-create-virtual-network?tabs=portal)**
