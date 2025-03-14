# Home Lab running Active Directory (Oracle VirtualBox)

In this lab, we will be setting up a virtualized network environment using Oracle VirtualBox, where we can create and configure a domain controller with Active Directory. This allows us to simulate a domain network with multiple virtual machines, including client machines that are part of the domain. By doing this, we can practice network managamenent tasks such as IP configuration, Active Directory setup, user creation, and domain joining, as well as learn how to configure networking (including DHCP and routing) in a controlled virtual environment

<img width="1000" src="https://github.com/user-attachments/assets/89ac5ab5-ab21-41be-b084-cd2d72c7546c" />

---

### Prerequisites
- Computer with stable internet connection
- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO) or [Windows 11 ISO](https://www.microsoft.com/en-us/software-download/windows11)
- [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)

---

# Table of Contents

- [](#)

---

## Collecting Files

<img width="1000" src="https://github.com/user-attachments/assets/89162725-d900-46d1-898e-f3a7bcd96f44" />

1. Once we have downloaded the files for our appropriate machine, I put them in a folder on my desktop to make them easier to find
     - Along with VirtualBox, we want to download the Extension Pack found on the same page

---

## Configuring Virtual Machine

1. Once VirtualBox is downloaded, open the program
     - We want to make a new machine
     - Name: DC (Domain Controller)
     - Version: Other Windows (64-bit)
     - Memory Size: 2048 MB
     - Create
<img width="1000" src="https://github.com/user-attachments/assets/6b86ccdd-a728-4771-be72-96f5d0cb1784" />

2. We want to further configure the system, so we'll go into the settings
     - Switch to Expert View
     - Shared Clipboard / Drag'n'Drop: Bidirectional
     - Processor(s): 4 CPUs
     - Adapter 1: NAT
     - Adapter 2: Internal Network
     - Save settings

3. Double click the VM to power it on
     - When prompted to select a virtual optical disk file, locate and choose the Windows Server 2019 ISO file
     - When prompted which operating system we want to install, use Windows Server 2019 Standard Evaluation (Desktop Experience)
     - Select Custom Install
     - The VM will restart on its own
     - It will prompt for a password that will be used to enter the system
     - Keyboard will not work, you will need to insert Ctrl+Alt+Delete to unlock
<img width="1000" src="https://github.com/user-attachments/assets/198af45b-d272-4da4-99a8-6832797f3b8d" />

4. Once inside, select Devices on the top left of our VM window and select Insert Guest Additions CD Image
     - This makes the VM run smoother
     - Open up File Explorer afterwards
     - This PC -> open CD Drive VirtualBox Guest Additions -> run amd4 addition
     - Shut down VM

---

## Configuring our Networks

1. Once we restart our VM it will prompt for the password we created earlier
2. On the bottom right, open network settings
     - Select Change Adapter Options
     - Notice there are two Ethernets
       
<img width="1000" src="https://github.com/user-attachments/assets/8b44f2ed-2367-435b-8fc9-0df7453eab9d" />

3. Determine which one is connected to the internet by looking at its details and rename accordingly
     - One adapter should be looking for a DHCP server to get an IP address from, but could not find one so it will have an Autoconfiguration IPv4 assigned to it, this will be the internal network.
     - Name accordingly
       
<img width="1000" src="https://github.com/user-attachments/assets/78ee9444-2430-4ea9-bcd5-021d9d0d282f" />

4. Go to the Internel networks Properties
     - Open Internet Protocol Version 4 (TCP/IPv4) properties
     - Use the following IP address from the diagram
         - IP address: 172.16.0.1 
         - Subnet mask: 255.255.255.0
         - Default gateway: <empty>
         - Preferred DNS server: 127.0.0.1 (loopback referring to themselves)

<img width="1000" src="https://github.com/user-attachments/assets/2338a213-3faf-4bb4-909c-0921b7794cb6" />

6. Next we are going to rename our PC
     - Right click on the start menu -> System
     - Rename this PC: DC (Domain Controller)
     - Restart VM
  
<img width="1000" src="https://github.com/user-attachments/assets/75675134-aba4-4728-a126-3326715d29d3" />

---

## Install Active Directory Domain Services

1. Once we have restarted our machine, access the Server Manager Dashboard
     - We want to Add roles and features (2)
     - Select the one server that we created
     - Check Active Directory Domain Services
     - Install

<img width="1000" src="https://github.com/user-attachments/assets/5eee7051-fffc-457c-9981-c331e3373142" />
    
2. Once installed, there will be a notification allowing us to Promote this server to a domain controller
     - We have installed the software for Active Directory Domain Services, but we have not created to domain yet
     - Select Promote
     - Add a new forest with a Root domain name: mydomain.com
     - Use the same password as before when creating another
     - Install
     - VM will automatically restart

![10](https://github.com/user-attachments/assets/75448720-f52d-4c3d-90ee-dcb5fb7c999e)
