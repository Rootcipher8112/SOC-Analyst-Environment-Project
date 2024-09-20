# SOC Analyst Environment Project

## Project Overview

This 30 day project, when completed, is designed to simulate a real-world **Security Operations Center (SOC) environment**, providing a practical demonstration of the key techniques, theories, and practices used in modern cybersecurity operations. By setting up and managing an ELK stack (Elasticsearch, Logstash, and Kibana), deploying various servers, and configuring centralized monitoring through tools like Elastic Agent and Fleet Server, this lab environment mimics the projects and workflows encountered by SOC analysts. The intent over the 30 days is to showcase the ability to manage security data across multiple endpoints, respond to security events, and maintain a secure network infrastructure.

This comprehensive environment not only highlights my technical proficiency in deploying and managing critical security tools but also demonstrates a deep understanding of cybersecurity principles. Through the project, I aim to demonstrate my hands-on experience with log management, system monitoring, and centralized policy enforcement, skills that are essential for ensuring the protection and resilience of enterprise-level IT infrastructures.



# Day 1: Logical Diagram Setup for SOC Analyst Environment

## Synopsis
Today, I am setting up the foundation for my SOC analyst environment by creating a logical diagram. The goal is to outline the architecture of the servers and network components that I will be working with throughout the project. I will use **draw.io** to build the diagram and ensure all elements are connected logically to represent the flow of data and network interactions.

## Step-by-Step Process

1. **Accessing draw.io**:  
   I started by navigating to the [draw.io website](https://www.draw.io) and created a new diagram. After setting up the workspace, I renamed the diagram to something relevant to the environment I’m building.

2. **Adding Server Icons**:  
   I searched for "server" in the icon menu and selected a standard server image. To create multiple servers, I duplicated the server icon until I had six servers, each representing a specific role in the environment.

3. **Labeling the Servers**:  
   Each of the six servers plays a unique role in the environment:
   - **Elastic and Kibana Server**
   - **Windows Server** (with RDP enabled)
   - **Ubuntu Server** (with SSH enabled)
   - **Fleet Server**
   - **OS Ticket Server**
   - **Command and Control (C2) Server** (I marked this in red for clarity)

4. **Creating a Virtual Private Cloud (VPC)**:  
   Since all the servers will be hosted in the cloud, I used the **Vulture** cloud provider and created a Virtual Private Cloud (VPC) to represent the isolated network where these servers will operate. I labeled the VPC in the diagram and placed it behind the servers for visual clarity.

5. **Connecting the Servers**:  
   I connected the servers with arrows to represent how data will flow between them:
   - The **Windows Server** is connected to the **Fleet Server**.
   - The **Ubuntu Server** is also connected to the **Fleet Server**.
   - The **Fleet Server** is linked to the **Elastic and Kibana Server**.
   - The **OS Ticket Server** is connected to **Elastic and Kibana** as well.
   
   I customized the arrows to represent different communication paths, such as log forwarding or alert management.

6. **Network Configuration**:  
   I added details about the private network, such as the IP range and subnet mask:
   - **IP Range**: 10.0.0.0/24
   - **Subnet Mask**: 255.255.255.0

7. **Internet Gateway and Analyst Laptop**:  
   I added an **Internet Gateway** to represent the connection to the wider internet. I also included two laptops:
   - The **SOC Analyst Laptop**, connected to the internet and the Elastic and Kibana dashboard via a web interface.
   - The **Attacker Laptop**, colored in red and connected to the **C2 Server** to simulate threat actor activity.

8. **Finalizing the Diagram**:  
   After ensuring all components were labeled and connected, I saved the diagram for future reference. This logical diagram provides a clear view of how the different elements of my SOC environment will interact.



![image](https://github.com/user-attachments/assets/e177f8bb-ef3b-4157-881b-3ea3d1b32d6b)



## Recap
I successfully created a logical diagram of the SOC environment. This diagram will serve as a roadmap for the infrastructure I’ll be working on throughout the project. Each element, from the servers to the private network, is now clearly mapped out, and I’m ready to start configuring the actual environment.



# Day 2: Synopsis to the ELK Stack

## Synopsis
Today, I focused on understanding the ELK Stack, a combination of **Elasticsearch**, **Logstash**, and **Kibana**, which plays a crucial role in security operations. These tools help in centralizing, managing, and visualizing logs, making it easier to analyze and respond to security events. By the end of this session, I have a better grasp of how the ELK Stack works and its benefits in a SOC environment.

## Elasticsearch
The "E" in ELK stands for **Elasticsearch**, which is a powerful database used to store and search logs. It can handle various types of logs, such as:
- Windows Event Logs
- Syslogs
- Firewall Logs

Elasticsearch uses **ESQL** (Elasticsearch Query Language), which is user-friendly and can be accessed via a **RESTful API** or through the Kibana web interface for those less familiar with API-based querying. Its flexibility allows me to search through vast amounts of data efficiently.

## Logstash
Next is **Logstash**, which acts as the processing pipeline. It collects telemetry from multiple sources, transforms it, and forwards it to Elasticsearch. Logstash is highly customizable and allows me to filter and manipulate the logs as needed. For example, I could configure it to only push specific event IDs, such as Windows Event ID 4624 (successful login events), into Elasticsearch, which helps reduce data ingestion costs.

Logstash can work with different telemetry collectors, including **Beats** (e.g., Filebeat, Metricbeat) and **Elastic Agent**. This flexibility gives me control over what data gets collected and how it is processed. Parsing logs in Logstash allows me to map data fields like source IP addresses to specific field names, which is essential for organizing and analyzing logs effectively.

## Kibana
The final component of the ELK Stack is **Kibana**, which provides a web-based interface for interacting with the data stored in Elasticsearch. Through Kibana, I can:
- Query logs using ESQL
- Create visualizations using **Kibana Lens**
- Set up dashboards and reports
- Monitor anomalies with built-in **machine learning** capabilities
- Use **Elastic Maps** for geospatial data

Kibana’s visual interface makes it easy to create dashboards that display key insights at a glance, which can be especially useful for reporting to stakeholders or executives.

## Benefits of the ELK Stack
After exploring the individual components, I identified several key benefits of using the ELK Stack in a SOC environment:
1. **Centralized Logging**: The ELK Stack helps centralize all logs in one place, making it easier to meet compliance requirements and investigate security incidents.
2. **Flexible Ingestion**: With Beats and Elastic Agent, I can choose which logs to ingest and how they are processed, offering flexibility in log management.
3. **Data Visualization**: Kibana allows me to create powerful dashboards that highlight critical security metrics, making data analysis easier and more actionable.
4. **Scalability**: The ELK Stack can be scaled to accommodate large environments, making it suitable for both small and enterprise-level deployments.
5. **Integration Ecosystem**: The ELK Stack supports numerous integrations and is used by many popular SIEM solutions, making it a valuable skill for transitioning between different security platforms.

## Recap
I explored the architecture and benefits of the ELK Stack, including Elasticsearch, Logstash, and Kibana. These tools offer powerful solutions for managing logs, visualizing data, and scaling infrastructure in a SOC environment. With this knowledge, I am better equipped to work with log management and data analysis in a security operations context.



# Day 3: Spinning Up an Elasticsearch Instance

## Synopsis
Today, I am focusing on spinning up my own **Elasticsearch** instance as part of building the ELK Stack for my SOC environment. By the end of this session, I will have a working instance of Elasticsearch running on a virtual machine in a cloud environment, ready for further configuration.

## Step-by-Step Process

1. **Setting Up a Virtual Private Cloud (VPC)**:
   - I started by heading over to [Vultr](https://www.vultr.com) and created an account.
   - Under the "Products" tab, I selected **Network** and clicked on **VPC 2.0** to create a Virtual Private Cloud (VPC).
   - After selecting a location (New York), I configured the IPv4 range to **10.0.0.0/24**, named the network, and added it to my account.

2. **Deploying a Virtual Machine**:
   - Next, I deployed a new virtual machine (VM) under the same location as the VPC (New York).
   - I chose **Ubuntu 20.04 LTS** as the operating system and selected a configuration with **4 CPUs** and **16GB of RAM**.
   - I disabled auto-backups and IPv6, selected the VPC I had just created, and took note of the VM's IP address.
   - I named the server **SOC1-ELK** and clicked **Deploy**. After a few minutes, the server status changed to "Running."

3. **Accessing the Virtual Machine**:
   - Once the VM was running, I accessed the console via SSH by opening **PowerShell** and running the following command:
     ```
     ssh root@<public_IP_address>
     ```
   - I logged in using the VM's IP address and root password, then updated the system by running:
     ```
     apt-get update && apt-get upgrade -y
     ```

4. **Installing Elasticsearch**:
   - To install Elasticsearch, I downloaded the package from the official website using:
     ```
     wget <download_link>
     ```
   - I confirmed the download was successful by listing the files in the directory.
   - I installed Elasticsearch by running:
     ```
     dpkg -i elasticsearch-<version>.deb
     ```
   - During installation, I made sure to save the **auto-configuration information** provided, including the built-in superuser password.

5. **Configuring Elasticsearch**:
   - I navigated to the **Elasticsearch configuration file** located at `/etc/elasticsearch/elasticsearch.yml`.
   - By default, Elasticsearch is only accessible locally. To allow access from my SOC analyst laptop, I modified the **network.host** setting by changing the IP address to the **private IP** of the VM.

6. **Securing Access with a Firewall**:
   - I created a firewall group in Vultr and restricted access to the VM using my public IP.
   - After applying the firewall rules, I updated the VM's firewall settings to use this newly created firewall group, ensuring tighter control over access to the Elasticsearch instance.

7. **Starting the Elasticsearch Service**:
   - I started the Elasticsearch service using the following commands:
     ```
     systemctl daemon-reload
     systemctl enable elasticsearch.service
     systemctl start elasticsearch.service
     ```
   - I confirmed the service was running by checking its status:
     ```
     systemctl status elasticsearch.service
     ```

## Recap
I successfully set up and secured an **Elasticsearch** instance on a cloud-hosted virtual machine. This instance will be a key component of my SOC environment, and in the next session, I’ll proceed with setting up **Kibana** to enable easier querying and visualization of logs via a web interface.



# Day 4: Setting Up and Installing Kibana

## Synopsis
Today, I focused on setting up **Kibana**, the final component of the ELK Stack that provides a web interface for querying and visualizing data from Elasticsearch. By the end of this session, I successfully installed and configured Kibana, ensuring it works in tandem with my Elasticsearch instance.

## Step-by-Step Process

1. **Downloading Kibana**:
   - I started by visiting the [Elastic website](https://www.elastic.co/downloads/kibana) to download Kibana.
   - After selecting the correct package for **Debian (x86 64-bit)**, I copied the download link and used `wget` in my SSH session to download Kibana.

2. **Installing Kibana**:
   - After the download completed, I installed Kibana by running:
     ```
     dpkg -i kibana-<version>.deb
     ```

3. **Configuring Kibana**:
   - I edited the Kibana configuration file, located at `/etc/kibana/kibana.yml`, to make two key changes:
     - **Server Port**: Uncommented the line but left it as the default `5601`.
     - **Server Host**: Uncommented the line and replaced `localhost` with the public IP address of my virtual machine.
   - After saving the changes, I enabled and started the Kibana service with:
     ```
     systemctl daemon-reload
     systemctl enable kibana.service
     systemctl start kibana.service
     ```

4. **Generating an Elasticsearch Enrollment Token**:
   - To integrate Kibana with Elasticsearch, I needed to generate an enrollment token. I navigated to the Elasticsearch directory and ran the following command to generate the token:
     ```
     /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token --scope kibana
     ```
   - I copied the token and saved it for later use.

5. **Configuring Firewall Rules**:
   - I needed to adjust firewall settings to allow traffic on Kibana's port (`5601`). First, I updated the firewall settings in Vultr by adding a rule to allow traffic from my IP on port `5601`.
   - I also modified the firewall on the Ubuntu virtual machine by running:
     ```
     ufw allow 5601
     ```

6. **Accessing Kibana**:
   - I accessed Kibana by opening a web browser and navigating to:
     ```
     http://<public_IP>:5601
     ```
   - I entered the enrollment token I had generated earlier, followed by the **username (elastic)** and the password provided during the Elasticsearch installation.

7. **Adding Encryption Keys**:
   - To avoid encryption key errors, I generated encryption keys using Kibana's built-in tools:
     ```
     /usr/share/kibana/bin/kibana-encryption-keys generate
     ```
   - I added these keys to Kibana’s keystore for secure storage:
     ```
     /usr/share/kibana/bin/kibana-keystore add <key_name>
     ```
   - After restarting Kibana, I confirmed the error was resolved.

## Recap
I successfully installed and configured **Kibana**, connecting it to my Elasticsearch instance and ensuring secure communication. This completes the setup of the ELK Stack. In the next session, I will configure a Windows Server to act as the target machine in my SOC environment.



# Day 5: Setting Up a Windows Server as a Target Machine

## Synopsis
Today, I set up a **Windows Server** in the cloud, which will act as the target machine for the SOC environment. By the end of this session, I had a fully functional Windows Server with **Remote Desktop Protocol (RDP)** exposed to the internet, ready to start collecting login attempts and other data for analysis.

## Step-by-Step Process

1. **Deploying the Windows Server**:
   - I logged into **Vultr** and selected **Deploy New Server**.
   - For the server type, I chose **Cloud Compute - Shared CPU** to keep costs low, and selected **New York** as the location to match the other components of the SOC environment.
   - I opted for **Windows Server 2022** as the operating system, choosing the $24/month option with 1 vCPU and 2GB of memory.

2. **Removing the Windows Server from the VPC**:
   - Initially, I had considered placing the Windows Server inside the **Virtual Private Cloud (VPC)** I had created earlier, but I decided against it for security reasons. If the Windows Server were compromised, I didn't want the attacker to have access to the rest of the network.
   - Instead, I kept the Windows Server outside the VPC, ensuring it had its own public IP address while maintaining the VPC for the other components, like **Elastic and Kibana**, **Fleet Server**, and **OS Ticket Server**.

3. **Naming the Server**:
   - I followed a specific naming convention: **SOC1-win-<username>**. For example, I named mine **SOC1-win-rootcipher8112**.
   - After setting up the name, I clicked **Deploy**.

4. **Accessing the Windows Server**:
   - Once the server status changed to "Running," I accessed it via the **View Console** option. I used the **Control-Alt-Delete** command to log in, entered the password provided in the Vultr dashboard, and signed into the Windows Server.
   
5. **Exposing RDP to the Internet**:
   - To ensure RDP was accessible, I copied the server’s public IP address and used **Remote Desktop** on my local machine to connect to the Windows Server.
   - The connection was successful, confirming that RDP is exposed to the internet and ready for use.

## Recap
By the end of today’s session, I successfully deployed a **Windows Server** in the cloud, exposed **RDP** to the internet, and ensured the server was operational. Over the next few hours, unsuccessful login attempts from the internet will begin to generate logs. In future sessions, I'll use these logs for analysis. In the next part, I’ll be setting up a **Fleet Server** to help manage configurations across multiple endpoints.



# Day 6: Understanding Fleet Server and Elastic Agent

## Synopsis
Today, I focused on two key components for managing logs and data collection in a SOC environment: the **Elastic Agent** and the **Fleet Server**. These tools allow me to manage endpoints in a centralized manner, simplifying the process of updating and configuring agents. By the end of this session, I had a clear understanding of how these components work and their benefits.

## Elastic Agent
The **Elastic Agent** is a unified agent used for collecting various types of data, such as logs and metrics, across multiple endpoints. It operates based on policies that can be easily updated to include new integrations or protections. These policies define what types of logs or metrics the agent should forward to **Elasticsearch** or **Logstash**.

There are two installation methods for Elastic Agents:
1. **Standalone Mode**: Each agent operates independently.
2. **Fleet Managed Mode**: Agents are centrally managed through a **Fleet Server**, which is the method I’ll be using for this project.

### Elastic Agent vs. Beats
Beats are lightweight data shippers designed for specific types of data, such as **Filebeat** for logs or **Metricbeat** for metrics. However, an Elastic Agent consolidates this functionality, acting as a single agent that can collect various types of data, eliminating the need to install multiple Beats on a single host. For most use cases, the Elastic Agent is sufficient.

## Fleet Server
The **Fleet Server** acts as a central management component that connects to Elastic Agents, allowing for centralized control over all endpoints. This makes it easy to:
- **Update Policies**: Modify what data the agents should collect or where it should be sent.
- **Add Integrations**: Quickly roll out new data ingestion integrations to all connected agents.
- **Manage Agent Versions**: Easily update agents when new versions are released or remove agents from the fleet.

Without a Fleet Server, I would have to manually update each agent’s configuration, which is inefficient, especially when managing a large number of endpoints. Using Fleet provides a streamlined way to ensure all agents are consistently configured and updated.

## Recap
I explored the **Elastic Agent** and **Fleet Server**, two crucial tools for efficiently managing logs and metrics in a SOC environment. The Elastic Agent simplifies data collection by consolidating multiple data sources, while the Fleet Server allows me to manage all endpoints from a single location. In the next session, I will install the Elastic Agent and set up my own Fleet Server to begin centralized management.



# Day 7: Installing Elastic Agent and Setting Up a Fleet Server

## Synopsis
Today, I installed an **Elastic Agent** on the Windows Server I created earlier and set up a **Fleet Server** for centralized management. By the end of this session, I had my Windows Server enrolled into the Fleet, allowing for easy monitoring and data collection across endpoints.

## Step-by-Step Process

1. **Creating the Fleet Server**:
   - I began by deploying a new server to act as the **Fleet Server**. I selected **Ubuntu 22.04** with 1 CPU and 4GB of memory, ensuring the server was added to the **Virtual Private Cloud (VPC)**.
   - Once the server was deployed, I accessed it through SSH and began updating the repositories by running:
     ```
     apt-get update && apt-get upgrade -y
     ```

2. **Setting Up the Fleet Server in the Web GUI**:
   - I logged into the **Kibana** web interface using the public IP address of the ELK Stack (Port 5601).
   - I navigated to **Fleet** under **Management**, selected **Add Fleet Server**, and followed the quick start setup. I entered the public IP of the new Fleet Server, ensuring it used **Port 8220** by default.
   - A Fleet Server policy was generated, which I copied and pasted into the Fleet Server's terminal to install and configure the **Elastic Agent** on it.

3. **Adjusting Firewall Rules**:
   - To allow the Fleet Server to communicate with Elasticsearch, I had to adjust firewall rules. I updated the firewall on both the ELK server and the Fleet Server to allow traffic on **Port 9200** for Elasticsearch and **Port 8220** for Fleet Server.
   - I also adjusted the **Ubuntu firewall (UFW)** to allow incoming connections on the necessary ports, ensuring proper communication between the components.

4. **Installing Elastic Agent on the Windows Server**:
   - After successfully configuring the Fleet Server, I turned my attention to the **Windows Server** created on Day 5. I accessed it via **Remote Desktop** and opened **PowerShell** as an administrator.
   - I pasted the command generated by Kibana to install the **Elastic Agent** on the Windows Server and waited for the installation to complete.
   - There were some initial errors with enrolling the agent due to network connectivity, but after troubleshooting firewall rules and updating the Fleet Server’s URL in Kibana, the agent was successfully enrolled.

5. **Enrolling the Windows Server in Fleet**:
   - Once the agent was installed, I verified that the Windows Server was successfully enrolled in the Fleet by checking the **Agents** section in Kibana. I saw the host **SOC1-win-rootcipher8112** listed, confirming the successful enrollment.

6. **Verifying Log Collection**:
   - I navigated to **Discover** in Kibana to verify that logs from the Windows Server were being collected. By searching for my server name, I was able to see logs, including **Event ID 4625** (failed logon attempts), indicating that the Elastic Agent was capturing authentication logs from the server.

## Recap
I successfully installed the **Elastic Agent** on my Windows Server and enrolled it in the **Fleet** for centralized management. Logs are now being collected, and I can manage the endpoint policies from one location. In the next session, I’ll set up **Sysmon** to monitor and log system activity on the Windows Server for deeper analysis.



# Day 8: Understanding Sysmon and Key Event IDs

## Synopsis
Today, I explored the importance of **Sysmon** (System Monitor), a tool from Microsoft's Sysinternals suite that provides detailed telemetry from Windows endpoints. Proper visibility into endpoints is critical for SOC analysts to effectively investigate potential compromises. By the end of this session, I had a better understanding of what Sysmon offers and its ability to capture vital events for security monitoring.

## What is Sysmon?
**Sysmon** is a free tool that helps monitor critical events on Windows machines, such as:
- Process Creations
- Network Connections
- File Creations

It works through event logging and can be customized via a configuration file to focus on specific events. With **Sysmon version 15.15**, there are **30 different event IDs** that can be tracked, providing detailed insights into endpoint activity. Sysmon’s capabilities, like logging process creation with command-line details and recording file hashes, make it a powerful tool for detecting and investigating suspicious behavior.

### Key Capabilities:
- **Process Creations**: Logs process details, including command line, parent processes, and file hashes. This helps in detecting malicious activity by correlating events using the **Process GUID**.
- **Network Connections**: Tracks source and destination IPs, ports, and the associated process. This is crucial for identifying potentially malicious connections, such as those from **Command and Control (C2)** processes.
- **Customizable Configuration**: Sysmon's behavior can be tailored through a configuration file to enable or disable specific event logging, such as network connections, which is disabled by default.

## Important Event IDs

Here are a few key event IDs provided by Sysmon that are valuable during an investigation:

1. **Event ID 1 (Process Creation)**: This tracks all new processes, their command lines, and file hashes, providing vital information for identifying suspicious activity. For example, if malware is executed, Sysmon will log this event.

2. **Event ID 3 (Network Connections)**: When enabled, this event tracks network connections, including source and destination IPs and ports. It's incredibly useful for tracking suspicious outbound connections initiated by malware.

3. **Event IDs 6, 7, 8 (Driver Load, Image Load, Create Remote Thread)**: These IDs help detect defense evasion techniques such as process injection, often used by attackers to bypass security tools.

4. **Event ID 10 (Process Access)**: This event is helpful in identifying potential credential theft attempts, particularly targeting the **LSASS** process, which stores credentials in memory.

5. **Event ID 22 (DNS Query)**: Monitoring DNS queries can reveal interesting patterns, including potential use of **Domain Generation Algorithms (DGA)**, which can indicate compromise.

## Recap
Sysmon provides essential telemetry for SOC analysts, allowing for detailed monitoring of endpoint activity. Understanding key event IDs like process creations and network connections enables more effective investigations and threat hunting. In the next session, I’ll demonstrate how to install Sysmon using a popular configuration to start collecting and analyzing Sysmon logs.



# Day 9: Installing and Configuring Sysmon on a Windows Server

## Synopsis
Today, I installed and configured **Sysmon** on the Windows Server created on Day 5. This tool is part of the Sysinternals suite and provides crucial telemetry for monitoring events such as process creation, network connections, and more. By the end of this session, I successfully confirmed that Sysmon was generating logs, setting the foundation for improved endpoint visibility.

## Step-by-Step Process

1. **Accessing the Windows Server**:
   - I logged into my **Vultr** account, retrieved the public IP address of my Windows Server, and connected via **Remote Desktop**. After logging in with the administrator credentials, I opened Microsoft Edge to begin the Sysmon installation.

2. **Downloading and Extracting Sysmon**:
   - I Googled **Sysmon** and downloaded version 15.15 from the official **Microsoft Learn** website.
   - After downloading the Sysmon zip file, I extracted it into a folder and verified the three binaries it contained.

3. **Downloading a Sysmon Configuration File**:
   - I downloaded a popular configuration file for Sysmon by searching for **Olaf's Sysmon configuration** on GitHub. I saved this **sysmon-config.xml** file in the Sysmon directory for later use.

4. **Installing Sysmon via PowerShell**:
   - Using **PowerShell** (run as Administrator), I navigated to the Sysmon directory and confirmed the presence of the binaries and configuration file.
   - I ran the following command to install Sysmon and apply the configuration file:
     ```
     sysmon64.exe -accepteula -i sysmon-config.xml
     ```
   - This command installed Sysmon and started it as a service, which I confirmed by checking the **Windows Services** and **Event Viewer**.

5. **Verifying Sysmon Logs**:
   - I reopened **Event Viewer**, navigated to **Applications and Services Logs > Microsoft > Windows > Sysmon**, and verified that Sysmon was generating event logs.
   - The first event ID I saw was **Event ID 3**, which captures **network connections**. I cross-referenced this with the official Sysmon documentation to confirm its meaning.

## Recap
I successfully installed and configured **Sysmon** on the Windows Server. I verified that it was logging events such as network connections, providing enhanced visibility into endpoint activity. In the next session, I will demonstrate how to push both Sysmon and **Microsoft Defender** logs into the **Elasticsearch** instance for centralized log analysis.



# Day 10: Ingesting Sysmon and Microsoft Defender Logs into Elasticsearch

## Synopsis
In today’s session, I demonstrated how to ingest both **Sysmon** and **Microsoft Defender** event logs from the Windows Server into an **Elasticsearch** instance. By the end of this video, I successfully confirmed that logs were being collected, providing essential visibility for further analysis.

## Step-by-Step Process

1. **Logging into Elasticsearch**:
   - I logged into the **Elasticsearch** instance and clicked on the "Add Integrations" button on the homepage. I searched for **Sysmon** and selected **Custom Windows Event Logs**, which allows ingestion from any Windows Event log channel.
   - After reviewing the fields, I clicked on "Add Custom Windows Event Logs" and created a new integration named **mydfir-win-sysmon**. I copied the **Sysmon** event log channel name from my Windows Server’s Event Viewer and added it to the integration.

2. **Adding Microsoft Defender Logs**:
   - I followed a similar process to ingest **Microsoft Defender** logs. After navigating to the **Defender** section in **Event Viewer**, I copied the operational log channel name and created a new integration named **mydfir-win-defender**.
   - I customized the integration to ingest specific **event IDs**, such as **1116 (malware detection)**, **1117 (protection action)**, and **50001 (real-time protection disabled)**, ensuring that only critical logs were ingested into Elasticsearch.

3. **Testing Log Ingestion**:
   - To test the **Microsoft Defender** integration, I disabled **real-time protection** on the Windows Server. I confirmed that **event ID 50001** was generated in **Event Viewer**.
   - After configuring both Sysmon and Microsoft Defender integrations, I used the Elasticsearch **Discover** tool to search for Sysmon logs by querying **windlog.event_id**. However, there were no results initially, prompting troubleshooting.

4. **Troubleshooting Log Ingestion**:
   - I noticed that **CPU and memory** values were showing as **N/A** in the agent details, which indicated that the agents could not communicate with Elasticsearch. To resolve this, I adjusted the firewall to allow incoming connections on **Port 9200**.
   - After restarting the **Elastic Agent** service and updating the firewall, I refreshed the agents and confirmed that both Sysmon and Microsoft Defender logs were being ingested successfully.

5. **Verifying Log Collection**:
   - I verified the ingestion of **Sysmon** logs by searching for **event ID 1**, which captures process creation events. I also searched for **event ID 50001** from Microsoft Defender, confirming that real-time protection logs were being collected.

## Recap
I successfully configured the ingestion of both **Sysmon** and **Microsoft Defender** logs into the **Elasticsearch** instance. This ensures centralized log collection for improved monitoring and analysis. In the next session, I will dive into a common attack scenario and explore the tools used to conduct it.



# Day 11: Understanding Brute Force Attacks

## Synopsis
In today's session, I explored the **Brute Force Attack**, a common cyber attack used in many environments. By the end of the session, I discussed the various tools attackers use to perform this attack and the best ways to defend against it.

## Step-by-Step Process

1. **What is a Brute Force Attack?**:
   - A **Brute Force Attack** is an attempt by attackers to gain unauthorized access to an account by trying every possible password combination until the correct one is found. Much like trying every combination on a keypad lock, attackers use automated tools to test various credentials against login systems.
   - This type of attack is common due to its ease of execution and widespread availability of exposed services like **Remote Desktop Protocol (RDP)**.

2. **Common Types of Brute Force Attacks**:
   - **Simple Brute Force**: This attack tries every possible combination of characters until the correct password is found. It is often automated and typically starts with basic combinations like "0000."
   - **Dictionary Attack**: Rather than testing every possible character combination, a **Dictionary Attack** uses a predefined list of common passwords and phrases, often sourced from credential dumps.
   - **Credential Stuffing**: Attackers use a list of known usernames and passwords from data breaches to attempt access to different accounts, banking on the fact that many users reuse passwords across sites.

3. **How to Protect Against Brute Force Attacks**:
   - **Long Passwords and Passphrases**: One of the most effective defenses is using strong, long passwords (preferably over 15 characters). Password managers can help generate and store these securely. Alternatively, **passphrases** like "PleaseSubscribeToMyChannel" offer security while remaining easy to remember.
   - **Multi-Factor Authentication (MFA)**: Enabling MFA adds an additional layer of protection. Even if an attacker gains access to your password, they’ll still need to provide a second authentication factor (e.g., SMS, email, or an authenticator app).
   - **Vigilance and Awareness**: Always be cautious of suspicious emails and requests to log into your accounts. Tools like **Have I Been Pwned** can alert you if your credentials have been compromised in a data breach.

4. **Common Tools Used for Brute Force Attacks**:
   - **Hydra**, **Hashcat**, and **John the Ripper** are some of the popular tools used by attackers to perform brute force attacks. If you’re interested in ethical hacking, these tools are available in **Kali Linux**, but it is important to only perform such actions on systems you own or have explicit permission to test.

## Recap
I discussed the basics of **Brute Force Attacks**, their various types, and how to defend against them using techniques like strong passwords, multi-factor authentication, and constant vigilance. I also introduced common tools used for brute force attacks, which can be explored for ethical hacking practices. In the next session, I’ll show you how to set up an **SSH server** to observe brute force attacks in real-time.



# Day 12: Setting Up an SSH Server and Monitoring Authentication Logs

## Synopsis
In today’s session, I set up an **SSH server** in the cloud and demonstrated how to review authentication logs in real-time. By the end of the session, I were able to observe **failed authentication attempts** and how to filter through these logs using command-line tools.

## Step-by-Step Process

1. **Deploying the SSH Server**:
   - I logged into **Vultr** and deployed a new **Ubuntu 24.04** cloud server. For the server name, I followed the same naming convention used in previous steps: **mydfir-linux-[handle]**.
   - After deployment, I accessed the server using **SSH** from **PowerShell**, updated the repositories, and prepared the environment for log monitoring.

2. **Locating Authentication Logs**:
   - On Ubuntu, all authentication logs are stored in the **/var/log/auth.log** file. This file contains logs related to authentication attempts, including both successful and failed logins.
   - I used the `cat` command to view the contents of the **auth.log**, but since the server was newly deployed, there wasn’t much activity yet.

3. **Monitoring Real-Time Logs**:
   - After waiting for about 30 minutes, I checked the **auth.log** again and started to see **failed password** attempts and **invalid user** entries. These were likely brute force attempts targeting the server, as discussed in **Day 11**.

4. **Filtering Log Entries**:
   - I used the **grep** command to filter out failed login attempts by searching for the keyword "failed" within the **auth.log** file.
   - To refine the search further, I filtered by the **root** user and isolated only the relevant entries, ignoring other invalid users.
   - Lastly, I demonstrated how to extract just the **IP addresses** from the failed login attempts using the **cut** command, which helped identify where the attempts originated from.

5. **Next Steps**:
   - I plan to leave the server running to collect more failed authentication attempts, continuing to observe how brute force attacks manifest in real-time.

## Recap
I successfully set up an **SSH server** in the cloud and monitored real-time authentication logs for failed login attempts, demonstrating how to filter and isolate important log details using command-line tools. In the next session, I’ll show how to install the **Elastic Agent** on the server to forward logs to the **Elasticsearch** instance for centralized log analysis.



# Day 13: Installing the Elastic Agent on the SSH Server

## Synopsis
In today’s session, I demonstrated how to install the **Elastic Agent** on the **SSH server** I set up on Day 12. By the end of this session, I successfully ingested logs from the SSH server into our **Elasticsearch** instance, allowing us to query for logs and monitor authentication events in real time.

## Step-by-Step Process

1. **Creating the Agent Policy**:
   - I logged into the **Elasticsearch** web GUI and navigated to **Fleet** under the management tab. From there, I created a new agent policy called **mydfir-linux-policy** to manage the logs coming from our SSH server.
   - This policy ensures that authentication logs from the **/var/log/auth.log** file on the Ubuntu server are sent to Elasticsearch for centralized analysis.

2. **Enrolling the SSH Server into Fleet**:
   - I connected to the SSH server via **PowerShell** and used the **Elastic Agent** command to enroll the server into the newly created policy.
   - Upon encountering a certificate error (x509), I resolved it by adding the `--insecure` flag to the command, which allowed the **Elastic Agent** to install successfully.

3. **Verifying Log Ingestion**:
   - Once the agent was successfully installed, I confirmed the data ingestion by navigating back to **Elasticsearch** and querying for logs related to the SSH server.
   - By filtering for specific fields, I was able to locate **authentication failures** and examine key details such as **IP addresses** and **message fields** that indicated failed login attempts.

4. **Enhanced Log Analysis**:
   - To make log analysis easier, I added the **message** field to the **Elasticsearch** table view, allowing for a clearer presentation of the logs, which included details like **authentication failures** and **IP addresses** of attempted brute force attacks.

## Recap
I successfully installed the **Elastic Agent** on the **SSH server** and confirmed that logs are being ingested into **Elasticsearch** for monitoring. This allows us to search for critical events such as **authentication failures** directly in the Elasticsearch GUI. In the next session, I’ll walk through setting up alerts for brute force attacks and creating a dashboard to visualize these attacks in real time.



# Day 14: Creating SSH Brute Force Alert and Dashboard

## Synopsis
In today’s session, I demonstrated how to create an alert for **SSH Brute Force activity** and a **dashboard** to visualize where these attacks are coming from. By the end of the session, I had successfully set up an alert system and a visualization map using **Elasticsearch** to monitor and respond to SSH brute force attempts.

## Step-by-Step Process

1. **Querying the SSH Server Logs**:
   - First, I logged into **Elasticsearch** and used the **Discover** tab to query the logs from our SSH server. I filtered the logs by **failed authentication attempts** and identified important fields, such as **failed attempts**, **username**, and **source IP**.
   - I saved this search query as **SSH Failed Activity** for future use and reference.

2. **Creating the SSH Brute Force Alert**:
   - With the query in place, I navigated to the **Alerts** tab to create a new **Search Threshold Rule** based on the saved query. This alert would trigger if there were more than **5 failed attempts** within a **5-minute window**, signaling potential brute force activity.
   - I saved the alert with the name **mydfir-ssh-brute-force-activity** along with my handle for the giveaway, ensuring the query and rule were configured correctly.

3. **Building the SSH Brute Force Dashboard**:
   - Next, I created a **visualization dashboard** to map the source of these SSH brute force attempts. Using the **Maps** feature in Elasticsearch, I built a map visualization that highlighted the geographic locations of failed authentication attempts based on **source IP**.
   - I configured the map to display **country names** and **ISO codes**, allowing me to track where the attacks were originating from in real time.

4. **Creating a Duplicate Dashboard for Successful Authentications**:
   - To complement the **failed authentication** dashboard, I created a duplicate to track **successful SSH authentications**. By adjusting the query to look for **accepted** logins, I was able to visualize any successful attempts alongside the failed ones, providing a fuller picture of SSH activity.

## Recap
I successfully set up an **SSH Brute Force alert** and a **dashboard** for visualizing brute force activity in Elasticsearch. This setup allows me to monitor and be alerted in real-time when SSH brute force attempts are detected and quickly analyze where these attacks are coming from. In the next session, I’ll go over a commonly used service in organizations that is also commonly abused. 



# Day 15: Understanding and Protecting Against RDP Abuse

## Synopsis
In today's session, I explored one of the most commonly abused protocols, **Remote Desktop Protocol (RDP)**, and its role in cyberattacks. By the end of this session, I gained a deeper understanding of **RDP**, how attackers abuse it, how to find exposed RDP services on the internet, and key steps to protect against RDP abuse.

## Key Points

1. **What is RDP?**
   - **RDP** is a protocol used to remotely connect to other machines, typically for administration or troubleshooting. It runs on **TCP Port 3389** and is widely used for remote work.
   - Its convenience also comes with risks, as **attackers often exploit exposed RDP services** to gain unauthorized access to systems.

2. **How Attackers Abuse RDP**:
   - Attackers search for **exposed RDP services** on the internet and try to brute force login credentials or use valid credentials obtained through phishing or credential dumps.
   - Once inside, attackers can perform **credential dumping** to move laterally within the network and achieve their objectives, such as deploying ransomware or data exfiltration.

3. **Identifying Exposed RDP Services**:
   - I explored two platforms, **Shodan** and **Censys**, that allow users to search for exposed RDP services by querying **Port 3389**. 
   - These tools can help security professionals identify whether their organization has exposed RDP services that need to be secured.

4. **How to Protect Against RDP Abuse**:
   - **Turn It Off**: Disable RDP on servers that don’t require it.
   - **Use Multi-Factor Authentication (MFA)**: Add an extra layer of security to protect accounts from unauthorized access.
   - **Restrict Access**: Implement firewall rules or place RDP access behind a **VPN** to restrict who can connect.
   - **Use Strong Passwords**: Enforce strong passwords with at least 15 characters, or use a **Privileged Access Management (PAM)** tool to generate one-time passwords.
   - **Disable Default Accounts**: Disable default administrator accounts to reduce the attack surface.

## Recap
I gained a solid understanding of how **RDP** works, why it’s a target for attackers, and what measures can be taken to protect against **RDP abuse**. With the right strategies—like turning off unnecessary services, using MFA, restricting access, and securing passwords—I can help reduce the risk of RDP-based attacks. In the next session, I’ll explore **RDP logs** generated by our **Windows Server** and create an alert for **RDP Brute Force attacks**.



# Day 16: Creating a Windows Brute Force Alert

## Synopsis
Today’s session focused on **Windows authentication logs** from our server created on **Day 5**. I also demonstrated how to create a **Brute Force alert** for the Windows server using the event code for failed authentication. By the end of this session, I had successfully set up a new alert that will notify us of any potential **RDP brute force** attempts.

## Key Points

1. **Windows Failed Authentication (Event ID 4625)**:
   - **Event ID 4625** documents every failed login attempt on a Windows machine.
   - To query these logs, I used **event.code: 4625** in Elastic’s **Discover** tool, filtering specifically for failed logins.
   - I added fields like **source IP address** and **username** to ensure accurate tracking of failed authentication attempts.

2. **Testing and Validation**:
   - I performed a failed login attempt using **RDP** on my Windows server to verify that the logs were being correctly captured.
   - Successful RDP logins use **Event ID 4624** and are typically associated with **logon type 10** (remote interactive).

3. **Creating the RDP Brute Force Alert**:
   - After verifying that failed RDP logins were being captured, I created a **Brute Force alert** using the **search threshold rule** feature in Elastic.
   - I set the alert to trigger when there are more than **5 failed attempts** within **5 minutes**, ensuring quick notification of brute force activity.

4. **Enhanced Alert Creation**:
   - While basic alerts can be created in **Discover**, I demonstrated how to create more detailed alerts using **Detection Rules** under the **Security** tab.
   - This method allows for custom queries, grouping by **source IP** and **username**, and includes more detailed information such as the source IP and affected user in the alert notification.

5. **Key Fields to Monitor**:
   - **Failed Attempts** (Event ID 4625)
   - **Source IP Address**
   - **Username**
   - **Logon Type** (Logon Type 3 for network-based, Logon Type 10 for remote interactive)

## Recap
I successfully created a **Brute Force alert** for my Windows server, tracking **Event ID 4625** for failed logins. I also demonstrated how to create more detailed detection rules for improved alerting. In the next session, I will create a **dashboard** to visualize where the failed authentication attempts are coming from, providing a more intuitive overview of potential brute force activity. 



# Day 17: Creating an RDP Activity Dashboard

## Synopsis
In this session, I demonstrated how to build a **dashboard** focusing on **RDP activity** from the Windows server created on **Day 5**. I created a visual map to track **failed and successful RDP authentication attempts** and visualized the data through tables displaying relevant information such as **username**, **source IP**, and **country of origin**. By the end of this session, I had a complete dashboard that provides a clear view of **RDP activity** on the server.

## Key Points

1. **RDP Failed Authentication Activity**:
   - The goal was to recreate a map similar to the one used for **SSH activity**, but focusing on **RDP failed attempts**.
   - The relevant query used was `event.code: 4625` (failed authentication) and **agent.name** (to filter our Windows server).
   - Using **Elastic Maps**, I added a layer to visualize **authentication attempts** geographically. For instance, a large number of failed attempts came from **Russia**.

2. **RDP Successful Authentication Activity**:
   - To capture successful RDP logins, I utilized **Event ID 4624**, focusing specifically on **logon type 10** (Remote Interactive).
   - After saving this query, I replicated the process from the failed attempts map, showing **successful login attempts** in the dashboard.

3. **Table Visualization**:
   - I complemented the map visualization with tables listing **username**, **source IP**, and **country of origin**.
   - This provided a quick and easy-to-read summary of both **failed** and **successful RDP attempts**.

4. **Final Dashboard**:
   - The dashboard now contains maps and tables for both **SSH** and **RDP activity**—including failed and successful attempts for both protocols.
   - This dashboard enables a clear, visual representation of authentication attempts and can quickly indicate where attacks are coming from.

## Recap
I successfully created a comprehensive **RDP dashboard** that visualizes **authentication attempts** from the Windows server. Using **Elastic Maps** and **Table visualizations**, the dashboard now offers insights into both **failed** and **successful attempts**, providing a high-level overview of **RDP activity** across different geographical locations. In the next session, I’ll dive into a **Command and Control** tactic using the **Mythic framework**, which attackers often use to establish unauthorized sessions.



# Day 18: Understanding Command and Control (C2) and Introduction to Mythic Framework

## Synopsis
In this session, I will begin to uncover the concept of **Command and Control (C2)**, explaining why it is a critical tactic for attackers and providing an overview of **common C2 tools** used in the wild. I also researched the **Mythic framework**, which I'll be using in this challenge. By the end of this session, you'll have a solid understanding of what **C2** is, why it's important for attackers, and some of the tools commonly used to establish C2 connections.

## Key Points

1. **What is Command and Control (C2)?**
   - **C2** refers to techniques used by attackers to communicate with systems they control within a victim's network. This allows them to perform malicious actions, such as **stealing credentials**, **moving laterally**, or **deploying ransomware**.
   - The **MITRE ATT&CK framework** lists **18 different techniques** attackers use to establish a C2 session. It’s a vital part of an attacker’s tactics, allowing them to reach their ultimate goal within a compromised environment.

2. **Why is C2 Important?**
   - **Establishing a C2** connection is crucial because it allows the attacker to **control the victim’s machine remotely**. This access is used for various purposes, including **information gathering**, **data exfiltration**, or **launching further attacks**.
   - Without a C2 channel, attackers would have limited capabilities to cause damage or move closer to their objectives.

3. **Common C2 Tools and Frameworks**:
   - **Metasploit**: A widely-used penetration testing framework that provides a variety of **exploits** and **auxiliary modules** for targeting vulnerable machines.
   - **Cobalt Strike**: A popular **commercial C2 tool** used for **adversary emulation**, though frequently abused in real-world attacks.
   - **Sliver**: An **open-source C2 framework** that provides similar capabilities to Cobalt Strike. Created by **Bishop Fox**, Sliver supports multiple C2 channels like **HTTP(S), DNS**, and **WireGuard**.
   - **Mythic**: The **C2 framework** I will use during this challenge. **Mythic** is built with **GoLang, Docker**, and a **web-based UI**. It supports multiple **agents** and **C2 profiles** for different communication methods.

4. **Mythic Framework**:
   - Mythic is a versatile C2 framework with a **web-based interface** that allows operators to **track payloads and callbacks**.
   - The framework uses **C2 profiles** for agents to manage communication between the compromised system and the Mythic server. 
   - Mythic currently supports **21 different agents**, making it a powerful tool for testing and adversary simulation.

## Recap
I explored the concept of **Command and Control (C2)** and why it's crucial for attackers. I also went over some of the most common **C2 tools** used by adversaries in the wild, with a focus on **Metasploit, Cobalt Strike, Sliver,** and **Mythic**. In the next session, I’ll begin setting up an attack on our **Windows server**, using **Mythic** to deploy an agent and establish a **C2 session**.



# Day 19: Creating an Attack Diagram for C2 Operations

## Synopsis
In this session, I focused on building an **attack diagram** using **draw.io** to map out the steps involved in attacking a target machine and establishing a **command and control (C2) session**. By the end of this session, I will demonstrate a clear understanding of the structure of an attack, including **initial access, discovery, defense evasion, execution, and exfiltration**. The diagram will help visualize the attack path and determine the steps to take once the target is compromised.

## Key Points

1. **Phase 1: Initial Access**
   - **Objective**: Perform an **RDP Brute Force** attack on the target **Windows Server** to gain initial access.
   - **Tools**: Use **Kali Linux** on the attacker's laptop to target the **Windows Server**.
   - **Success**: If successful, the attacker will gain **authenticated access** to the server.

2. **Phase 2: Discovery**
   - **Objective**: Run basic **discovery commands** on the compromised server to gather information about the system and network.
   - **Commands**: `whoami`, `ipconfig`, `net user`, `net group`.
   - **Goal**: Understand the permissions and environment to plan further actions.

3. **Phase 3: Defense Evasion**
   - **Objective**: **Disable Windows Defender** on the compromised server to evade detection and ensure successful execution of malicious actions.
   - **Method**: Use the RDP session to disable **Windows Defender**.

4. **Phase 4: Execution**
   - **Objective**: Download and execute the **Mythic agent** from the **Mythic C2 server** using **PowerShell Invoke Expression** (`IEX`).
   - **Goal**: Establish a communication channel between the **Windows Server** and the **Mythic C2 server**.

5. **Phase 5: Command and Control**
   - **Objective**: Establish a **C2 session** with the compromised Windows server.
   - **Goal**: Once the **C2 session** is active, the attacker can issue commands to the server.

6. **Phase 6: Exfiltration**
   - **Objective**: Use the **C2 session** to download a fake **passwords.txt** file from the compromised server to simulate data exfiltration.
   - **Outcome**: This phase simulates the final step of many attacks—stealing sensitive data.

## Attack Diagram
  ![Untitled Diagram drawio](https://github.com/user-attachments/assets/a390fb1d-e21d-4077-bed6-d4142e2d916e)
##

## Attack Diagram Overview
I used **draw.io** to create an attack diagram, which mapped out the following:
- **Attacker laptop** (Kali Linux) performs **RDP Brute Force** on the **Windows Server**.
- Once the attacker gains **initial access**, they perform **discovery** using basic commands.
- The attacker disables **Windows Defender** to avoid detection, followed by downloading the **Mythic agent**.
- The **Mythic agent** establishes a **C2 session** with the **Mythic C2 server**.
- Finally, the attacker performs **exfiltration** by downloading the fake `passwords.txt` file.

## Recap
I created an **attack diagram** to visualize the steps of our **C2 operation** using **Mythic**. The diagram outlined the key phases: **initial access**, **discovery**, **defense evasion**, **execution**, **C2 establishment**, and **exfiltration**. In the next session, I will begin setting up the **Mythic C2 server** and prepare to execute the attack on our **Windows Server**.



