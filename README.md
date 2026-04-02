# Wireshark Network Lab

## Overview

This lab explores how to use Wireshark to capture, filter, and analyze network traffic. The focus is on understanding packet behavior, identifying protocols, and troubleshooting common network scenarios.

### Skills Learned

**Network Traffic Analysis** – Captured and analyzed live packet data to understand how devices communicate across a network.

**Wireshark Packet Inspection** – Identified and interpreted different protocol packets using color coding, packet details, and structured analysis.

**Packet Filtering & Analysis** – Applied capture and display filters to isolate specific traffic such as DNS, TCP, and HTTP for deeper investigation.

**DNS Analysis** – Tracked domain name queries and responses, identified failed lookups, and matched requests using transaction IDs.

**TCP Fundamentals & Troubleshooting** – Analyzed TCP 3-way handshake (SYN, SYN-ACK, ACK) to verify successful connections and identify failures.

**TCP Retransmission Analysis** – Detected retransmissions and understood TCP backoff behavior caused by unstable or interrupted network connections.

**HTTP Traffic Inspection** – Examined HTTP GET requests and server responses, including interpreting status codes like 200 OK.

**Time-Based Network Analysis** – Used delta time and time reference features to identify delays, interruptions, and packet timing patterns.

**Name Resolution Techniques** – Converted IP addresses and port numbers into human-readable names to simplify packet analysis.

### Tools Used

- Wireshark
- Dumpcap
- Command Prompt (Windows CLI)
- Wi-Fi Network Interface

### Lab Setup

Wireshark shows all interfaces with activity.

<img width="500" height="400" alt="Screenshot 2026-03-29 204609" src="https://github.com/user-attachments/assets/37dc91a6-55ee-4af9-be89-df8d26050168" />

Using Manage Interfaces, unnecessary interfaces can be hidden to reduce clutter.

<img width="500" height="454" alt="Screenshot 2026-03-29 205803" src="https://github.com/user-attachments/assets/09dacb69-5a76-481d-bd2f-42b25c0acb4c" />

## Lab

### 1. Filtering Traffic

**Capture Filters**: Limit what type of packets get recorded

**Display Filters**: Filter and analyze through packets that have already been recorded 

Important: Be careful being too specific when creating capture filters as you may miss a trace adhering to a different type of protocol.

Utilized Port 53 to capture UDP DNS packets. However, some TCP packets are also present as Wireshark switches from UDP to TCP if the packet is too big (usually over 512 bytes).

<img width="600" height="400" alt="Screenshot 2026-03-30 113940" src="https://github.com/user-attachments/assets/a63a5f6b-bb74-416b-b123-ac11eaa6e697" />

For display filters you can run a simple open the capture for you connection and apply filters at the top of the screen

Here I filtered anything to and from a specific address using ip.addr == address. If the address specified is present in either the source of the destination the packet will be displayed.

<img width="600" height="500" alt="Screenshot 2026-03-30 114909" src="https://github.com/user-attachments/assets/6663ed92-794d-4188-827d-d0f2fa838d37" />

Used right click conversation filtering to see IPv4 conversation between two endpoints. Right click filtering is faster and puts you at less risk of making a mistake as opposed to typing out the script in the filter line.

<img width="500" height="400" alt="Screenshot 2026-03-30 115608" src="https://github.com/user-attachments/assets/daf37cbb-851e-40e3-8d35-7bd85a9540eb" />

Select a packet > UDP protocol > Right click > Prepare as filter > ...and selected allows us to preview all UDP packets from that conversation.

<img width="500" height="400" alt="Screenshot 2026-03-30 220448" src="https://github.com/user-attachments/assets/419dbf05-efed-4b81-8d03-f0c48d74943e" />

**Filtering by Text**: Searches for keywords inside packets containing certain words useful for enabling identification of HTTP requests, credentials, and detecting unusual traffic.

Display filter which showed all packets that contained the word “google” using the script frame matches “google”. 

<img width="600" height="500" alt="Screenshot 2026-03-30 221640" src="https://github.com/user-attachments/assets/385ffa23-2403-4454-8c6b-86a927d8a957" />


### 2. Capturing Packets with Dumpcap

Dumpcap is a command-line tool that captures packets more effeciently than the Wireshark GUI.

Script which shows interfaces on my system including the index number of the interfaces which we often need to call on.

<img width="500" height="400" alt="Screenshot 2026-03-29 214529" src="https://github.com/user-attachments/assets/f40fb9dc-9828-456e-b418-b06cb252706b" />

Used this script which includes the index number to view details of specific interface. Dumpcap provides the name of the interface, the location of temp folder and packets captured.

<img width="400" height="250" alt="Screenshot 2026-03-29 214901" src="https://github.com/user-attachments/assets/7acb389b-5e1f-4d2e-893e-3e7c4b6505eb" />

Script to save the interface and then write traffic to a ring buffer with a file size of 500000 megabytes and maximum number of 10 files.

<img width="400" height="200" alt="Screenshot 2026-03-29 220024" src="https://github.com/user-attachments/assets/9ef6b846-0bfb-444f-999e-00d9e7b21b80" />


### 3. Name Resolution

**Purpose**: Name resolution resolves numerical network numbers (IP addresses, MAC addresses, and port numbers) to human readable names.

**Configuration**
Enabled in preferences:
- Resolve transport names
- Resolve network (IP) addresses

Domains appear instead of raw IPs and services appear instead of port numbers.

<img width="600" height="520" alt="Screenshot 2026-03-30 230647" src="https://github.com/user-attachments/assets/4b7b8e0d-b585-4a42-9b7e-1c3a62e45726" />

Resolved domains can be viewed through Statistics > Resolved adresses

<img width="500" height="400" alt="Screenshot 2026-03-30 231314" src="https://github.com/user-attachments/assets/1d8d8a7c-b02a-4ec7-86ea-98722295ec15" />

Manually configured my host to have the name “Client” and one destination to be “Gateway” using right click > Edit the Resolved Name

<img width="443" height="302" alt="Screenshot 2026-03-30 231957" src="https://github.com/user-attachments/assets/779e78b7-ec8f-4d7f-acc7-8060242b58e1" />


### 4. Time Analysis  

**Time Column***: Can be used to identify delays, detect anomolies or track event timing.

To set a packet as a reference Right-click → Set Time Reference counts time elapsed in refrence to that packet.

<img width="500" height="400" alt="Screenshot 2026-03-31 175616" src="https://github.com/user-attachments/assets/982c5d57-4ace-4eea-8941-7c063c30d8f0" />

**Time Since Previous Frame**: Column(time since previous frame in this TCP stream) that can be added which measures the time between packets within a specific TCP conversation. Finding this time would require some extra calculation if we were only looking at Delta time.

Example: Packet 8 showed ~1 second delay from previous packet this helps identify slow or interrupted communication.

<img width="500" height="400" alt="Screenshot 2026-03-31 181833" src="https://github.com/user-attachments/assets/4c8c1065-168b-4b9c-b853-bd1dd03f5f95" />

### 5. Simulated Troubleshooting Scenario 

#### Step 1. DNS Analysis

**Set Up**: For this scenario, I ran two domains on my wifi, one real and one fake to see what kind of response we would get from the DNS server.

Filtered for DNS packet using the script "dns"

**Fake Domain Observations**

For the fake website the standard query tells us “No such name” meaning that the website does not exist.

<img width="500" height="400" alt="Screenshot 2026-04-01 145434" src="https://github.com/user-attachments/assets/892dc3a2-de9b-45c6-8a12-81027aaa78ff" />




- Set up a static IP address for both the Windows server and client by onfiguring the network adapter settings to match the IP configuration identified via ipconfig.

- Same configuration was then repeated on the Windows 11 client

Static addressing prevents domain authentication failures caused by dynamic IP changes.

#### Attaching to the Domain

- In the settings for the client I went to Access work or school/Join this device to a local active directory domain/enter domain name/enter username and password/restart the computer

- To confirm the client was attached to the domain I went to Active Direcetory Users and Computer and confirmed the computer(WS01) was listed

<img width="500" height="400" alt="Screenshot 2026-02-28 212015" src="https://github.com/user-attachments/assets/3adf9ab8-a765-47ba-94c7-2923bb1d7ab3" />

### 3. Orginazational Units and Access Control Design

Overview: The approach of creating orginizational groups and even groups within them allows us to add role-based permissions to an entire group instead of having to modify each user indiviudally promoting efficiency.

- Created three diffrent Orginazational Units (OUs) reprsenting diffrent departments (Engineering,Management,IT) to reprsent a real work enviornment
- Designed the OU structure to reflect departmental seperation when it comes to administration access
- Created a security group called Engineering Share to grant that specific group access to shared resources

  Added:
- 2 users from the Engineering Department
- 1 user from management(simulating cross-department collaboration)

<img width="500" height="400" alt="Screenshot 2026-03-01 123542" src="https://github.com/user-attachments/assets/befe4f31-69d9-4d30-a640-c0172de113c1" />

- Assigned permissions to the shared resource using the Engineering Share security group

Under advanced security settings we can ensure that Engineering Share group (besides administrator) is the only one who recieves said permissions

<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/b513b326-ec44-4a9f-884f-c07cfc72b42d" />


### 4. Group Policy Objects

Overview: Group Policy Objects (GPOs) allow administrators to centrally manage system configurations and user environments within an Active Directory domain. The GPO that will be created in this step is a desktop background for the engineering department.

#### Policy Configuration

- Created a custom white desktop wallpaper

- Stored the wallpaper inside the NETLOGON share, which allows domain-wide access to files used in policy deployment

<img width="500" height="400" alt="movingwallpaperintoNETLOGON" src="https://github.com/user-attachments/assets/616e9400-0ff6-49ac-b6f4-b89d3f3cb236" />

- Copied the file path so it can be pasted into the policy setting later

#### Policy Deployment

- Created a GPO called "SetEngineeringBackground" for the engineering OU 

<img width="400" height="400" alt="Setengbackground" src="https://github.com/user-attachments/assets/dd165971-ff2e-4ecf-b2ab-3f0b4846547a" />

- Located the policy setting for desktop wallaper where I was able paste the file path from earlier and enable it

<img width="500" height="400" alt="pasted the filepath for wallpaper policy" src="https://github.com/user-attachments/assets/fee5689b-6e2f-4796-816b-a031c84b43e1" />

- Logged into domain accounts belonging to Engineering users and verified that they did have the wallpaper active

- Users outside the Engineering OU were not affected by the policy, confirming that the GPO was correctly applied.

### 5. Ticket Processing

Overview: In IT environments, help desk requests are typically managed through a ticketing system. A ticket represents a user request or issue that requires administrative action, such as password resets, account unlocks, or access permissions. For this step I processed a hypothetical ticket coming from a manager regarding the desktop wallpaper that set in step 4. 

Ticket:

<img width="500" height="400" alt="Screenshot 2026-03-01 143933" src="https://github.com/user-attachments/assets/e3a07205-3e15-4486-b721-9fa6ba135f5f" />

- Under tools in the domain server I accessed group policy management and located the setting that prevents the changing of the background under the engineering organizational unit

<img width="500" height="400" alt="LockingDesktopWallpaper" src="https://github.com/user-attachments/assets/705f46a1-d0c5-4172-8b13-9e6bd6363a08" />

- To confirm the policy was enforced, I logged in as one of the users from the engineering department. I was completely prhohibited from changing the background which can be seen below

<img width="500" height="400" alt="VirtualBox_Windows for HD_01_03_2026_15_01_46" src="https://github.com/user-attachments/assets/d154671b-e65e-4b2d-a445-1a18bb235f56" />

### 6. Task Automation with Powershell

Overview: Task automation is beneficial because instead of doing tasks manually in AD such as creating users manually like we did earlier in step 2, we can write a script that does all of it for us. For this step I utilized Powershell to create an individual user for the domain.

Powershell script to create the user: 

<img width="500" height="400" alt="Automating Task w Powershell" src="https://github.com/user-attachments/assets/609051e2-f99b-435b-8330-94382dd5a8c8" />

### 7. Resetting AD Passwords

Overview: Account lockout policies are commonly used in enterprise environments to prevent unauthorized access and brute-force login attempts. In this lab, an account lockout threshold was configured through Group Policy to simulate a security control used in real-world Active Directory environments.

After three failed login attempts, the user account becomes locked and requires administrative intervention to regain access.

#### Manual Process

First I took a manual approach to for the password reset by doing it through the domain controller, this method is less efficient but demonstrated what goes on behind the scenes.

- Configured an Account Lockout Policy under Group Policy Management which locked users out after 3 incorrect login attempts

<img width="500" height="400" alt="Account Lockout Threshold" src="https://github.com/user-attachments/assets/5f142a17-ab5a-4906-9c0d-170c5aa0c1ac" />

- Switched over to the Windows Client and triggered the lockout using one of the created users

- Reset the user's password through Active Directory Users and Computers

<img width="500" height="400" alt="pass reset" src="https://github.com/user-attachments/assets/dac8b6de-c0cd-4afb-981f-e545af45e4e6" />

#### Automated Process

For a more effecient practice I repeated this password reset using Powershell automation. 

##### Powershell Script

<img width="500" height="400" alt="reset password PS" src="https://github.com/user-attachments/assets/6d0ab820-9148-46cf-aeb1-56f4c668fd07" />

PowerShell allows administrators to quickly reset passwords and unlock accounts without navigating the Active Directory which lowers the risk for any configuration errors

##### Applies to both manual and automated process:

- In a real-world enviornment the user would be notified of the temporary password through a secure form of communication
- The user would then login on thier end using that password where they would then prompted to reset it again using thier own password













  
