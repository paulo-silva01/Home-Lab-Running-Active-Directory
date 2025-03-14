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

<img width="1000" alt="10" src="https://github.com/user-attachments/assets/75448720-f52d-4c3d-90ee-dcb5fb7c999e" />
<img width="1000" src="https://github.com/user-attachments/assets/26cec4bc-576c-4505-915b-4d3cd36336f6" />

3. We will want to create our own dedicated domain admin account instead of using the built in admin account
     - In the start menu -> Windows Administrative Tools -> Active Directory Users and Computers
     - mydomain.com, right click and create a new Organizational Unit titled _ADMINS
     - uncheck the Protect container

<img width="1000" alt="14" src="https://github.com/user-attachments/assets/f3241b77-6e1e-4a03-9b05-e83f178d7bff" />

4. Right click on the new Organizational Unit (_ADMINS)
     - Create new User
     - User your name
     - Normal naming convention would be [a(admin)-firstletteroffirstname-lastname]
     - Uncheck user cannot change password, and select Password never expires
     - Finish

<img width="1000" src="https://github.com/user-attachments/assets/8f6bd8d4-c4dd-4319-a82b-796ff3726752" />

5. Right click on the new account under your name
     - Go to properties
     - Under the Member of tab, select Add...
     - Enter Domain Admins object name
     - Check
     - Apply
     - Now we have our own Domain Admin Account

<img width="1000" src="https://github.com/user-attachments/assets/4184590e-a117-46bb-b70f-523ca7ca5166" />

6. Now when we sign out, and sign in user Other user, use the admin name you created
     - Notice how it signs in to: MYDOMAIN
  
<img width="1000" src="https://github.com/user-attachments/assets/efa266e3-c6e8-4804-a749-6723a3a473d0" />

---

## Install Remote Access Server and Network Access Translation

1. We want to install RAS/NAT on our domain controller to allow our Windows 10 client to be on a private virtual network but still have access to the internet through the domain controller
     - Add roles and features in our Server Manager Dashboard
     - Server Roles: Remote Access
     - Role Services: Routing
     - Install
<img width="1000" src="https://github.com/user-attachments/assets/f5f01dc7-2dce-4ccc-accb-0947545de145" />
<img width="1000" src="https://github.com/user-attachments/assets/63413a58-3dc5-4946-be8f-73c5b575629f" />

2. Next, go to Tools on the Dashboard, and select Routing and Remote Access
     - Right click DC (local), select Configure and Enable routing and Remote Access
     - We want to install NAT to allow internal clients to connect to the Internet using one public IP address
     - Select Use this public interface to connect to the internet
     - Choose the internet option that we renamed
     - Now our clients are able to access the internet once we setup the DHCP

<img width="1000" src="https://github.com/user-attachments/assets/ccecb280-f9cb-4368-be11-5bf1b47293f1" />

---

## Configuring our DHCP Server

1. Add roles and features in our Server Manager Dashboard
     - Server Roles: DHCP Server
     - Install

<img width="1000" alt="21" src="https://github.com/user-attachments/assets/51bca55a-c50e-41a4-bfa7-50e53d0b6f4b" />

2. Go to Tools in the Server Manager Dashboard
     - Click DHCP
     - Expanding dc.mydomain.com you will notice IPv4 and IPv6 are both down (red indicator)
     - Right click IPv4 -> New Scope...
     - The name will be the Range displayed in our diagram (172.16.0.100-200)
     - Start IP address: 172.16.0.100
     - End IP address: 172.16.0.200
     - Subnetmask: 255.255.255.0
     - Lease Duration: 8 days
     - Router (Default Gateway): 172.16.0.1
            - Since the clients are using internet through the domain controller, we will assign their default gateway to be the same as the domain controllers internet NIC
     - Click Add
     - Finish
     - Right click dc.mydomain.com and select Authorize, when you refresh the servers should be up and running

<img width="1000" src="https://github.com/user-attachments/assets/eb06654c-8882-414f-9764-9802504806ca" />
<img width="1000" src="https://github.com/user-attachments/assets/91331564-6d63-47c1-9cf1-a5ea2aac17b9" />

4. Next we need to make a configuration that allows us to browse the internet through the domain controller
     - Configure this local server on the Server Manager Dashboard
     - Turn off IE Enhanced Security Configuration

<img width="1000" src="https://github.com/user-attachments/assets/a76edb92-4110-46d7-bdea-db2518ad9cac" />

---

## Using PowerShell to Create Users

1. Click on Start -> Windows PowerShell -> Windows PowerShell ISE -> More -> Run as administrator
     - We are going to take 1000+ names into a document and run the following script
  
<img width="1000" src="https://github.com/user-attachments/assets/ef8930cc-7b4c-45ed-b9a9-24dce1079cd4" />

2. This PowerShell script is used to create new Active Directory (AD) users based on a list of names from a file (names.txt)
     - [$PASSWORD_FOR_USERS] sets the password for all users to Password1
     - [$USER_FIRST_LAST_LIST] reads the names of users from a file called names.txt. Each line in the file should contain a first and last name
     - [$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force] Converts the plain-text password (Password1) into a secure string to use with the New-AdUser cmdlet. This is a requirement for securely setting passwords in AD
     - [New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false] creates a new Organizational Unit (OU) in Active Directory called _USERS. The option -ProtectedFromAccidentalDeletion $false means the OU is not protected from accidental deletion, so it can be deleted later if necessary
     - The [foreach ($n in $USER_FIRST_LAST_LIST)] block Loops through each name in the list ($USER_FIRST_LAST_LIST). It splits each name into a first name and a last name (assuming the names are space-separated). Converts both the first and last names to lowercase. Creates a username by taking the first letter of the first name and appending the entire last name (both in lowercase)
     - AccountPassword: Sets the user's password to the secure string
     - GivenName: Sets the first name of the user
     - Surname: Sets the last name of the user
     - DisplayName: Sets the display name (often used for email or in the AD interface)
     - Name: Sets the full name of the user
     - EmployeeID: Uses the username for the employee ID
     - PasswordNeverExpires: Sets the user's password to never expire
     - Path: Specifies the path (location) in AD where the user is created. In this case, the _USERS OU is used
     - Enabled: Enables the user account immediately upon creation
