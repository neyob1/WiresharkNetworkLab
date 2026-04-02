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

### 5. Simulated Troubleshooting Scenarios

#### Step 1. DNS Analysis

**Set Up**: For this scenario, I ran two domains on my wifi, one real(google.com) and one fake to see what kind of response we would get from the DNS server.

Filtered for DNS packet using the script "dns"

**Fake Domain Observations**

For the fake website the standard query tells us “No such name” meaning that the website does not exist.

<img width="1000" height="800" alt="Screenshot 2026-04-01 145434" src="https://github.com/user-attachments/assets/892dc3a2-de9b-45c6-8a12-81027aaa78ff" />

**Real Domain Observations**

For the fake website example we had the query and it's response right after one another for easy locating and analysis. However that is not always the case, packets don’t always arrive in neat pairs.

To match the google.com query with its response we must use a script (dns contains “google”) in our display filter that is filtering for all DNS packets related to google.com.

Now we can clearly see a DNS packet related to google.com with a Transaction ID of 0xde33, transaction id is crucial for locating a response.

<img width="500" height="400" alt="Screenshot 2026-04-01 151537" src="https://github.com/user-attachments/assets/9e87b98d-1428-406c-9c52-288457ca7c72" />

Back in our display filter we can use dns.id == 0xde33, which gives a clean two packet display of the query and its response.

<img width="500" height="400" alt="Screenshot 2026-04-01 153326" src="https://github.com/user-attachments/assets/87495c63-bf3b-4fa1-a6b7-b228e89b4a90" />

A request and a reply indicates that website does and the query was sucessful.

#### Step 2. TCP Analysis

TCP uses a 3-way handshake:
SYN → Request connection
SYN-ACK → Acknowledgment
ACK → Connection established

Use script "tcp" in our display filter and click on a packet. For this example I used the website LinkedIn. 

**Confirming the Handshake**

Once I located a TCP packet related to LinkedIn, I followed the stream and analayzed three packets. One that included SYN, a second that included SYN, ACK and a third one that had ACK. All three being present tells me that the connection worked and the site loaded manually.

<img width="600" height="500" alt="Screenshot 2026-04-01 164125" src="https://github.com/user-attachments/assets/cd645aa6-eba9-4e97-b798-30d9433b901e" />

**TCP Retransmissions** 

Occurs when the sender in this case the computer does not receive acknowledgment in time. This leads to TCP resending the packet until it does receive acknowledgment.

TCP retransmitted packets can be viewed by using the script “tcp.analysis.retransmission”

<img width="500" height="400" alt="Screenshot 2026-04-01 165940" src="https://github.com/user-attachments/assets/1f0705ee-3108-4f93-9b63-af1407802af8" />


**Simulation**

- Opened a website
- Disabled Wi-Fi for 2-3 seconds while loading

Observation: Packets were retransmitted with gaps that were exponentially increasing due to TCP backoff. This occurs when the network is unstable or dropping packets. 

**TCP Backoff**: Every time time a packet is sent and is met with no acknowledgement, the time between retransmission doubles.

<img width="500" height="400" alt="Screenshot 2026-04-01 171153" src="https://github.com/user-attachments/assets/34c1e3be-b3d1-44e5-8899-8ad43cfe3f43" />

#### Step 3: HTTP Analysis 

**Filter:** “http” and it will show all packets regarding communication between a client server about a specific action or resource.

**GET Request:** Packets labeled “GET”  indicate that resources are being requested from the server

<img width="700" height="600" alt="Screenshot 2026-04-01 184612" src="https://github.com/user-attachments/assets/e33ad814-dd68-4103-bd25-bebdded05a8e" />

Http request packet > middle panel > expand Hypertext Transfer Protocol shows us exactly what the client requested from the server

<img width="700" height="600" alt="Screenshot 2026-04-01 184552" src="https://github.com/user-attachments/assets/30a642b1-1c48-4229-8bf2-a5505a97c9f1" />

**Server Response:** Directly after the GET request will be server responses such as 200 OK, 404 Not Found, 500 Internal Server Error which shows how the server responded to the request

The 200 OK = success indicates the client's request was successfully received, understood, and accepted by the server.

<img width="682" height="400" alt="Screenshot 2026-04-01 184525" src="https://github.com/user-attachments/assets/c0390b8c-d284-4f9b-af63-58faa23b4542" />

### Key Takeaways

- Wireshark provides deep visibility into network behavior
- Filtering is essential for effective analysis
- DNS, TCP, and HTTP work together in most web activity
- Timing and retransmissions are critical for troubleshooting
- Command-line tools like dumpcap improve performance under load























  
