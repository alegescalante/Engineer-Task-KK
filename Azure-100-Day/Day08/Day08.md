# Day 8: Attach Managed Disk to Azure Virtual Machine

An existing VM named `nautilus-vm` and a managed disk named `nautilus-disk` already exist in the `East US` region.

Attach the disk `nautilus-disk` to the VM `nautilus-vm` as a data disk.

Ensure the disk is attached to the VM `nautilus-vm`.

Make sure that the virtual machine initialization has been completed before submitting this task.

## Add a data disk

Azure Docs to be followed: [Attach an existing disk](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal#attach-an-existing-disk)

- Sign in to the `Azure portal`.
- Search for and select `Virtual machines`.
- Select a virtual machine from the list.

  <img width="1083" height="237" alt="image" src="https://github.com/user-attachments/assets/ba3ec441-d024-4221-9af9-793a01adc11a" />

- On the virtual machine pane, From `Settings`, choose `Disk`.
  
  <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/25a9bffe-b6d7-469a-8359-e3d67985e931" />

- Under `Data disk`, choose `Attach existing disk` and select the existing disk.

  <img width="1060" height="211" alt="image" src="https://github.com/user-attachments/assets/bdc98ba0-6b78-4bb5-a64d-27306923f608" />
