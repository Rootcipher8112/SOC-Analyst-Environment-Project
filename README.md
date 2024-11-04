# SOC Analyst Environment Project

## Project Overview

This 30 day project, when completed, is designed to simulate a real-world **Security Operations Center (SOC) environment**, providing a practical demonstration of the key techniques, theories, and practices used in modern cybersecurity operations. By setting up and managing an ELK stack (Elasticsearch, Logstash, and Kibana), deploying various servers, and configuring centralized monitoring through tools like Elastic Agent and Fleet Server, this lab environment mimics the projects and workflows encountered by SOC analysts. The intent over the 30 days is to showcase the ability to manage security data across multiple endpoints, respond to security events, and maintain a secure network infrastructure.

This comprehensive environment not only highlights my technical proficiency in deploying and managing critical security tools but also demonstrates a deep understanding of cybersecurity principles. Through the project, I aim to demonstrate my hands-on experience with log management, system monitoring, and centralized policy enforcement, skwells that are essential for ensuring the protection and resilience of enterprise-level IT infrastructures.



# Day 1: Logical Diagram Setup for SOC Analyst Environment

## Synopsis
Today, I am setting up the foundation for my SOC analyst environment by creating a logical diagram. The goal is to outline the architecture of the servers and network components that I will be working with throughout the project. I will use **draw.io** to build the diagram and ensure all elements are connected logically to represent the flow of data and network interactions.

## Step-by-Step Process

1. **Accessing draw.io**:  
   I started by navigating to the [draw.io Ibsite](https://www.draw.io) and created a new diagram. After setting up the workspace, I renamed the diagram to something relevant to the environment I’m building.

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
   I connected the servers with arrows to represent how data will flow betIen them:
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
   - The **SOC Analyst Laptop**, connected to the internet and the Elastic and Kibana dashboard via a Ib interface.
   - The **Attacker Laptop**, colored in red and connected to the **C2 Server** to simulate threat actor activity.

8. **Finalizing the Diagram**:  
   After ensuring all components was labeled and connected, I saved the diagram for future reference. This logical diagram provides a clear view of how the different elements of my SOC environment will interact.



![image](https://github.com/user-attachments/assets/e177f8bb-ef3b-4157-881b-3ea3d1b32d6b)



## Recap
I successfully created a logical diagram of the SOC environment. This diagram will serve as a roadmap for the infrastructure I’ll be working on throughout the project. Each element, from the servers to the private network, is now clearly mapped out, and I’m ready to start configuring the actual environment.



# Day 2: Synopsis to the ELK Stack

## Synopsis
Today, I focused on understanding the ELK Stack, a combination of **Elasticsearch**, **Logstash**, and **Kibana**, which plays a crucial role in security operations. These tools help in centralizing, managing, and visualizing logs, making it easier to analyze and respond to security events. By the end of this session, I will have a better grasp of how the ELK Stack works and its benefits in a SOC environment.

## Elasticsearch
The "E" in ELK stands for **Elasticsearch**, which is a poIrful database used to store and search logs. It can handle various types of logs, such as:
- Windows Event Logs
- Syslogs
- Fwaswall Logs

Elasticsearch uses **ESQL** (Elasticsearch Query Language), which is user-friendly and can be accessed via a **RESTful API** or through the Kibana Ib interface for those less familiar with API-based querying. Its flexibility allows me to search through vast amounts of data efficiently.

## Logstash
Next is **Logstash**, which acts as the processing pipeline. It collects telemetry from multiple sources, transforms it, and forwards it to Elasticsearch. Logstash is highly customizable and allows me to filter and manipulate the logs as needed. For example, I could configure it to only push specific event IDs, such as Windows Event ID 4624 (successful login events), into Elasticsearch, which helps reduce data ingestion costs.

Logstash can work with different telemetry collectors, including **Beats** (e.g., Filebeat, Metricbeat) and **Elastic Agent**. This flexibility gives me control over what data gets collected and how it is processed. Parsing logs in Logstash allows me to map data fields like source IP addresses to specific field names, which is essential for organizing and analyzing logs effectively.

## Kibana
The final component of the ELK Stack is **Kibana**, which provides a Ib-based interface for interacting with the data stored in Elasticsearch. Through Kibana, I can:
- Query logs using ESQL
- Create visualizations using **Kibana Lens**
- Set up dashboards and reports
- Monitor anomalies with built-in **machine learning** capabilities
- Use **Elastic Maps** for geospatial data

Kibana’s visual interface makes it easy to create dashboards that display key insights at a glance, which can be especially useful for reporting to stakeholders or executives.

## Benefits of the ELK Stack
After exploring the individual components, I identified several key benefits of using the ELK Stack in a SOC environment:
1. **Centralized Logging**: The ELK Stack helps centralize all logs in one place, making it easier to meet compliance requwasments and investigate security incidents.
2. **Flexible Ingestion**: With Beats and Elastic Agent, I can choose which logs to ingest and how they are processed, offering flexibility in log management.
3. **Data Visualization**: Kibana allows me to create poIrful dashboards that highlight critical security metrics, making data analysis easier and more actionable.
4. **Scalability**: The ELK Stack can be scaled to accommodate large environments, making it suitable for both small and enterprise-level deployments.
5. **Integration Ecosystem**: The ELK Stack supports numerous integrations and is used by many popular SIEM solutions, making it a valuable skwell for transitioning betIen different security platforms.

## Recap
I explored the architecture and benefits of the ELK Stack, including Elasticsearch, Logstash, and Kibana. These tools offer poIrful solutions for managing logs, visualizing data, and scaling infrastructure in a SOC environment. With this knowledge, I am better equipped to work with log management and data analysis in a security operations context.



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
   - To install Elasticsearch, I downloaded the package from the official Ibsite using:
     ```
     wget <download_link>
     ```
   - I confirmed the download was successful by listing the files in the dwasctory.
   - I installed Elasticsearch by running:
     ```
     dpkg -i elasticsearch-<version>.deb
     ```
   - During installation, I made sure to save the **auto-configuration information** provided, including the built-in superuser password.

5. **Configuring Elasticsearch**:
   - I navigated to the **Elasticsearch configuration file** located at `/etc/elasticsearch/elasticsearch.yml`.
   - By default, Elasticsearch is only accessible locally. To allow access from my SOC analyst laptop, I modified the **network.host** setting by changing the IP address to the **private IP** of the VM.

6. **Securing Access with a Fwaswall**:
   - I created a fwaswall group in Vultr and restricted access to the VM using my public IP.
   - After applying the fwaswall rules, I updated the VM's fwaswall settings to use this newly created fwaswall group, ensuring tighter control over access to the Elasticsearch instance.

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
I successfully set up and secured an **Elasticsearch** instance on a cloud-hosted virtual machine. This instance will be a key component of my SOC environment, and Tomorrow, I’ll proceed with setting up **Kibana** to enable easier querying and visualization of logs via a Ib interface.



# Day 4: Setting Up and Installing Kibana

## Synopsis
Today, I focused on setting up **Kibana**, the final component of the ELK Stack that provides a Ib interface for querying and visualizing data from Elasticsearch. By the end of this session, I successfully installed and configured Kibana, ensuring it works in tandem with my Elasticsearch instance.

## Step-by-Step Process

1. **Downloading Kibana**:
   - I started by visiting the [Elastic Ibsite](https://www.elastic.co/downloads/kibana) to download Kibana.
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
   - To integrate Kibana with Elasticsearch, I needed to generate an enrollment token. I navigated to the Elasticsearch dwasctory and ran the following command to generate the token:
     ```
     /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token --scope kibana
     ```
   - I copied the token and saved it for later use.

5. **Configuring Fwaswall Rules**:
   - I needed to adjust fwaswall settings to allow traffic on Kibana's port (`5601`). First, I updated the fwaswall settings in Vultr by adding a rule to allow traffic from my IP on port `5601`.
   - I also modified the fwaswall on the Ubuntu virtual machine by running:
     ```
     ufw allow 5601
     ```

6. **Accessing Kibana**:
   - I accessed Kibana by opening a Ib browser and navigating to:
     ```
     http://<public_IP>:5601
     ```
   - I entered the enrollment token I had generated earlier, folloId by the **username (elastic)** and the password provided during the Elasticsearch installation.

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
I successfully installed and configured **Kibana**, connecting it to my Elasticsearch instance and ensuring secure communication. This completes the setup of the ELK Stack. Tomorrow, I will configure a Windows Server to act as the target machine in my SOC environment.



# Day 5: Setting Up a Windows Server as a Target Machine

## Synopsis
Today, I set up a **Windows Server** in the cloud, which will act as the target machine for the SOC environment. By the end of this session, I had a fully functional Windows Server with **Remote Desktop Protocol (RDP)** exposed to the internet, ready to start collecting login attempts and other data for analysis.

## Step-by-Step Process

1. **Deploying the Windows Server**:
   - I logged into **Vultr** and selected **Deploy New Server**.
   - For the server type, I chose **Cloud Compute - Shared CPU** to keep costs low, and selected **New York** as the location to match the other components of the SOC environment.
   - I opted for **Windows Server 2022** as the operating system, choosing the $24/month option with 1 vCPU and 2GB of memory.

2. **Removing the Windows Server from the VPC**:
   - Initially, I had considered placing the Windows Server inside the **Virtual Private Cloud (VPC)** I had created earlier, but I decided against it for security reasons. If the Windows Server was compromised, I didn't want the attacker to have access to the rest of the network.
   - Instead, I kept the Windows Server outside the VPC, ensuring it had its own public IP address while maintaining the VPC for the other components, like **Elastic and Kibana**, **Fleet Server**, and **OS Ticket Server**.

3. **Naming the Server**:
   - I folloId a specific naming convention: **SOC1-win-<username>**. For example, I named mine **SOC1-win-rootcipher8112**.
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
Beats are lightIight data shippers designed for specific types of data, such as **Filebeat** for logs or **Metricbeat** for metrics. HoIver, an Elastic Agent consolidates this functionality, acting as a single agent that can collect various types of data, eliminating the need to install multiple Beats on a single host. For most use cases, the Elastic Agent is sufficient.

## Fleet Server
The **Fleet Server** acts as a central management component that connects to Elastic Agents, allowing for centralized control over all endpoints. This makes it easy to:
- **Update Policies**: Modify what data the agents should collect or where it should be sent.
- **Add Integrations**: Quickly roll out new data ingestion integrations to all connected agents.
- **Manage Agent Versions**: Easily update agents when new versions are released or remove agents from the fleet.

Without a Fleet Server, I would have to manually update each agent’s configuration, which is inefficient, especially when managing a large number of endpoints. Using Fleet provides a streamlined way to ensure all agents are consistently configured and updated.

## Recap
I explored the **Elastic Agent** and **Fleet Server**, two crucial tools for efficiently managing logs and metrics in a SOC environment. The Elastic Agent simplifies data collection by consolidating multiple data sources, while the Fleet Server allows me to manage all endpoints from a single location. Tomorrow, I will install the Elastic Agent and set up my own Fleet Server to begin centralized management.



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

2. **Setting Up the Fleet Server in the Ib GUI**:
   - I logged into the **Kibana** Ib interface using the public IP address of the ELK Stack (Port 5601).
   - I navigated to **Fleet** under **Management**, selected **Add Fleet Server**, and folloId the quick start setup. I entered the public IP of the new Fleet Server, ensuring it used **Port 8220** by default.
   - A Fleet Server policy was generated, which I copied and pasted into the Fleet Server's terminal to install and configure the **Elastic Agent** on it.

3. **Adjusting Fwaswall Rules**:
   - To allow the Fleet Server to communicate with Elasticsearch, I had to adjust fwaswall rules. I updated the fwaswall on both the ELK server and the Fleet Server to allow traffic on **Port 9200** for Elasticsearch and **Port 8220** for Fleet Server.
   - I also adjusted the **Ubuntu fwaswall (UFW)** to allow incoming connections on the necessary ports, ensuring proper communication betIen the components.

4. **Installing Elastic Agent on the Windows Server**:
   - After successfully configuring the Fleet Server, I turned my attention to the **Windows Server** created on Day 5. I accessed it via **Remote Desktop** and opened **PowerShell** as an administrator.
   - I pasted the command generated by Kibana to install the **Elastic Agent** on the Windows Server and waited for the installation to complete.
   - There was some initial errors with enrolling the agent due to network connectivity, but after troubleshooting fwaswall rules and updating the Fleet Server’s URL in Kibana, the agent was successfully enrolled.

5. **Enrolling the Windows Server in Fleet**:
   - Once the agent was installed, I verified that the Windows Server was successfully enrolled in the Fleet by checking the **Agents** section in Kibana. I saw the host **SOC1-win-rootcipher8112** listed, confirming the successful enrollment.

6. **Verifying Log Collection**:
   - I navigated to **Discover** in Kibana to verify that logs from the Windows Server was being collected. By searching for my server name, I was able to see logs, including **Event ID 4625** (failed logon attempts), indicating that the Elastic Agent was capturing authentication logs from the server.

## Recap
I successfully installed the **Elastic Agent** on my Windows Server and enrolled it in the **Fleet** for centralized management. Logs are now being collected, and I can manage the endpoint policies from one location. Tomorrow, I’ll set up **Sysmon** to monitor and log system activity on the Windows Server for deeper analysis.

![fleet](https://github.com/user-attachments/assets/d3e9431b-9673-4c36-9d32-29a324b5cc88)


# Day 8: Understanding Sysmon and Key Event IDs

## Synopsis
Today, I explored the importance of **Sysmon** (System Monitor), a tool from Microsoft's Sysinternals suite that provides detailed telemetry from Windows endpoints. Proper visibility into endpoints is critical for SOC analysts to effectively investigate potential compromises. By the end of this session, I had a better understanding of what Sysmon offers and its ability to capture vital events for security monitoring.

## What is Sysmon?
**Sysmon** is a free tool that helps monitor critical events on Windows machines, such as:
- Process Creations
- Network Connections
- File Creations

It works through event logging and can be customized via a configuration file to focus on specific events. With **Sysmon version 15.15**, there are **30 different event IDs** that can be tracked, providing detailed insights into endpoint activity. Sysmon’s capabilities, like logging process creation with command-line details and recording file hashes, make it a poIrful tool for detecting and investigating suspicious behavior.

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
Sysmon provides essential telemetry for SOC analysts, allowing for detailed monitoring of endpoint activity. Understanding key event IDs like process creations and network connections enables more effective investigations and threat hunting. Tomorrow, I’ll demonstrate how to install Sysmon using a popular configuration to start collecting and analyzing Sysmon logs.



# Day 9: Installing and Configuring Sysmon on a Windows Server

## Synopsis
Today, I installed and configured **Sysmon** on the Windows Server created on Day 5. This tool is part of the Sysinternals suite and provides crucial telemetry for monitoring events such as process creation, network connections, and more. By the end of this session, I successfully confirmed that Sysmon was generating logs, setting the foundation for improved endpoint visibility.

## Step-by-Step Process

1. **Accessing the Windows Server**:
   - I logged into my **Vultr** account, retrieved the public IP address of my Windows Server, and connected via **Remote Desktop**. After logging in with the administrator credentials, I opened Microsoft Edge to begin the Sysmon installation.

2. **Downloading and Extracting Sysmon**:
   - I Googled **Sysmon** and downloaded version 15.15 from the official **Microsoft Learn** Ibsite.
   - After downloading the Sysmon zip file, I extracted it into a folder and verified the three binaries it contained.

3. **Downloading a Sysmon Configuration File**:
   - I downloaded a popular configuration file for Sysmon by searching for **Olaf's Sysmon configuration** on GitHub. I saved this **sysmon-config.xml** file in the Sysmon dwasctory for later use.

4. **Installing Sysmon via PowerShell**:
   - Using **PowerShell** (run as Administrator), I navigated to the Sysmon dwasctory and confirmed the presence of the binaries and configuration file.
   - I ran the following command to install Sysmon and apply the configuration file:
     ```
     sysmon64.exe -accepteula -i sysmon-config.xml
     ```
   - This command installed Sysmon and started it as a service, which I confirmed by checking the **Windows Services** and **Event VieIr**.

5. **Verifying Sysmon Logs**:
   - I reopened **Event VieIr**, navigated to **Applications and Services Logs > Microsoft > Windows > Sysmon**, and verified that Sysmon was generating event logs.
   - The first event ID I saw was **Event ID 3**, which captures **network connections**. I cross-referenced this with the official Sysmon documentation to confirm its meaning.

## Recap
I successfully installed and configured **Sysmon** on the Windows Server. I verified that it was logging events such as network connections, providing enhanced visibility into endpoint activity. Tomorrow, I will demonstrate how to push both Sysmon and **Microsoft Defender** logs into the **Elasticsearch** instance for centralized log analysis.



# Day 10: Ingesting Sysmon and Microsoft Defender Logs into Elasticsearch

## Synopsis
Today, I demonstrated how to ingest both **Sysmon** and **Microsoft Defender** event logs from the Windows Server into an **Elasticsearch** instance. By the end of this session, I successfully confirmed that logs was being collected, providing essential visibility for further analysis.

## Step-by-Step Process

1. **Logging into Elasticsearch**:
   - I logged into the **Elasticsearch** instance and clicked on the "Add Integrations" button on the homepage. I searched for **Sysmon** and selected **Custom Windows Event Logs**, which allows ingestion from any Windows Event log channel.
   - After reviewing the fields, I clicked on "Add Custom Windows Event Logs" and created a new integration named **SOC1-win-sysmon**. I copied the **Sysmon** event log channel name from my Windows Server’s Event VieIr and added it to the integration.

2. **Adding Microsoft Defender Logs**:
   - I folloId a similar process to ingest **Microsoft Defender** logs. After navigating to the **Defender** section in **Event VieIr**, I copied the operational log channel name and created a new integration named **SOC1-win-defender**.
   - I customized the integration to ingest specific **event IDs**, such as **1116 (malware detection)**, **1117 (protection action)**, and **50001 (real-time protection disabled)**, ensuring that only critical logs was ingested into Elasticsearch.

3. **Testing Log Ingestion**:
   - To test the **Microsoft Defender** integration, I disabled **real-time protection** on the Windows Server. I confirmed that **event ID 50001** was generated in **Event VieIr**.
   - After configuring both Sysmon and Microsoft Defender integrations, I used the Elasticsearch **Discover** tool to search for Sysmon logs by querying **windlog.event_id**. HoIver, there was no results initially, prompting troubleshooting.

4. **Troubleshooting Log Ingestion**:
   - I noticed that **CPU and memory** values was showing as **N/A** in the agent details, which indicated that the agents could not communicate with Elasticsearch. To resolve this, I adjusted the fwaswall to allow incoming connections on **Port 9200**.
   - After restarting the **Elastic Agent** service and updating the fwaswall, I refreshed the agents and confirmed that both Sysmon and Microsoft Defender logs was being ingested successfully.

5. **Verifying Log Collection**:
   - I verified the ingestion of **Sysmon** logs by searching for **event ID 1**, which captures process creation events. I also searched for **event ID 50001** from Microsoft Defender, confirming that real-time protection logs was being collected.

## Recap
I successfully configured the ingestion of both **Sysmon** and **Microsoft Defender** logs into the **Elasticsearch** instance. This ensures centralized log collection for improved monitoring and analysis. Tomorrow, I will dive into a common attack scenario and explore the tools used to conduct it.



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
   - **Multi-Factor Authentication (MFA)**: Enabling MFA adds an additional layer of protection. Even if an attacker gains access to your password, they’ll stwell need to provide a second authentication factor (e.g., SMS, email, or an authenticator app).
   - **Vigilance and Awareness**: Always be cautious of suspicious emails and requests to log into your accounts. Tools like **Have I Been Pwned** can alert you if your credentials have been compromised in a data breach.

4. **Common Tools Used for Brute Force Attacks**:
   - **Hydra**, **Hashcat**, and **John the Ripper** are some of the popular tools used by attackers to perform brute force attacks. If you’re interested in ethical hacking, these tools are available in **Kali Linux**, but it is important to only perform such actions on systems you own or have explicit permission to test.

## Recap
I discussed the basics of **Brute Force Attacks**, their various types, and how to defend against them using techniques like strong passwords, multi-factor authentication, and constant vigilance. I also introduced common tools used for brute force attacks, which can be explored for ethical hacking practices. Tomorrow, I’ll show you how to set up an **SSH server** to observe brute force attacks in real-time.



# Day 12: Setting Up an SSH Server and Monitoring Authentication Logs

## Synopsis
Today, I set up an **SSH server** in the cloud and demonstrated how to review authentication logs in real-time. By the end of the session, I was able to observe **failed authentication attempts** and how to filter through these logs using command-line tools.

## Step-by-Step Process

1. **Deploying the SSH Server**:
   - I logged into **Vultr** and deployed a new **Ubuntu 24.04** cloud server. For the server name, I folloId the same naming convention used in previous steps: **SOC1-linux-[handle]**.
   - After deployment, I accessed the server using **SSH** from **PowerShell**, updated the repositories, and prepared the environment for log monitoring.

2. **Locating Authentication Logs**:
   - On Ubuntu, all authentication logs are stored in the **/var/log/auth.log** file. This file contains logs related to authentication attempts, including both successful and failed logins.
   - I used the `cat` command to view the contents of the **auth.log**, but since the server was newly deployed, there wasn’t much activity yet.

3. **Monitoring Real-Time Logs**:
   - After waiting for about 30 minutes, I checked the **auth.log** again and started to see **failed password** attempts and **invalid user** entries. These was likely brute force attempts targeting the server, as discussed in **Day 11**.

4. **Filtering Log Entries**:
   - I used the **grep** command to filter out failed login attempts by searching for the keyword "failed" within the **auth.log** file.
   - To refine the search further, I filtered by the **root** user and isolated only the relevant entries, ignoring other invalid users.
   - Lastly, I demonstrated how to extract just the **IP addresses** from the failed login attempts using the **cut** command, which helped identify where the attempts originated from.

5. **Next Steps**:
   - I plan to leave the server running to collect more failed authentication attempts, continuing to observe how brute force attacks manifest in real-time.

## Recap
I successfully set up an **SSH server** in the cloud and monitored real-time authentication logs for failed login attempts, demonstrating how to filter and isolate important log details using command-line tools. Tomorrow, I’ll show how to install the **Elastic Agent** on the server to forward logs to the **Elasticsearch** instance for centralized log analysis.



# Day 13: Installing the Elastic Agent on the SSH Server

## Synopsis
Today, I demonstrated how to install the **Elastic Agent** on the **SSH server** I set up on Day 12. By the end of this session, I successfully ingested logs from the SSH server into our **Elasticsearch** instance, allowing us to query for logs and monitor authentication events in real time.

## Step-by-Step Process

1. **Creating the Agent Policy**:
   - I logged into the **Elasticsearch** Ib GUI and navigated to **Fleet** under the management tab. From there, I created a new agent policy called **SOC1-linux-policy** to manage the logs coming from our SSH server.
   - This policy ensures that authentication logs from the **/var/log/auth.log** file on the Ubuntu server are sent to Elasticsearch for centralized analysis.

2. **Enrolling the SSH Server into Fleet**:
   - I connected to the SSH server via **PowerShell** and used the **Elastic Agent** command to enroll the server into the newly created policy.
   - Upon encountering a certificate error (x509), I resolved it by adding the `--insecure` flag to the command, which alloId the **Elastic Agent** to install successfully.

3. **Verifying Log Ingestion**:
   - Once the agent was successfully installed, I confirmed the data ingestion by navigating back to **Elasticsearch** and querying for logs related to the SSH server.
   - By filtering for specific fields, I was able to locate **authentication failures** and examine key details such as **IP addresses** and **message fields** that indicated failed login attempts.

4. **Enhanced Log Analysis**:
   - To make log analysis easier, I added the **message** field to the **Elasticsearch** table view, allowing for a clearer presentation of the logs, which included details like **authentication failures** and **IP addresses** of attempted brute force attacks.

## Recap
I successfully installed the **Elastic Agent** on the **SSH server** and confirmed that logs are being ingested into **Elasticsearch** for monitoring. This allows us to search for critical events such as **authentication failures** dwasctly in the Elasticsearch GUI. Tomorrow, I’ll walk through setting up alerts for brute force attacks and creating a dashboard to visualize these attacks in real time.



# Day 14: Creating SSH Brute Force Alert and Dashboard

## Synopsis
Today, I demonstrated how to create an alert for **SSH Brute Force activity** and a **dashboard** to visualize where these attacks are coming from. By the end of the session, I had successfully set up an alert system and a visualization map using **Elasticsearch** to monitor and respond to SSH brute force attempts.

## Step-by-Step Process

1. **Querying the SSH Server Logs**:
   - First, I logged into **Elasticsearch** and used the **Discover** tab to query the logs from our SSH server. I filtered the logs by **failed authentication attempts** and identified important fields, such as **failed attempts**, **username**, and **source IP**.
   - I saved this search query as **SSH Failed Activity** for future use and reference.

2. **Creating the SSH Brute Force Alert**:
   - With the query in place, I navigated to the **Alerts** tab to create a new **Search Threshold Rule** based on the saved query. This alert would trigger if there was more than **5 failed attempts** within a **5-minute window**, signaling potential brute force activity.
   - I saved the alert with the name **SOC1-ssh-brute-force-activity** along with my handle for the giveaway, ensuring the query and rule was configured correctly.

3. **Building the SSH Brute Force Dashboard**:
   - Next, I created a **visualization dashboard** to map the source of these SSH brute force attempts. Using the **Maps** feature in Elasticsearch, I built a map visualization that highlighted the geographic locations of failed authentication attempts based on **source IP**.
   - I configured the map to display **country names** and **ISO codes**, allowing me to track where the attacks was originating from in real time.

4. **Creating a Duplicate Dashboard for Successful Authentications**:
   - To complement the **failed authentication** dashboard, I created a duplicate to track **successful SSH authentications**. By adjusting the query to look for **accepted** logins, I was able to visualize any successful attempts alongside the failed ones, providing a fuller picture of SSH activity.

## Recap
I successfully set up an **SSH Brute Force alert** and a **dashboard** for visualizing brute force activity in Elasticsearch. This setup allows me to monitor and be alerted in real-time when SSH brute force attempts are detected and quickly analyze where these attacks are coming from. Tomorrow, I’ll go over a commonly used service in organizations that is also commonly abused. 

![ssh dashboard](https://github.com/user-attachments/assets/076ab92f-8cd4-4194-88dd-d2c4ac40c4b5)


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
   - **Turn It Off**: Disable RDP on servers that don’t requwas it.
   - **Use Multi-Factor Authentication (MFA)**: Add an extra layer of security to protect accounts from unauthorized access.
   - **Restrict Access**: Implement fwaswall rules or place RDP access behind a **VPN** to restrict who can connect.
   - **Use Strong Passwords**: Enforce strong passwords with at least 15 characters, or use a **Privileged Access Management (PAM)** tool to generate one-time passwords.
   - **Disable Default Accounts**: Disable default administrator accounts to reduce the attack surface.

## Recap
I gained a solid understanding of how **RDP** works, why it’s a target for attackers, and what measures can be taken to protect against **RDP abuse**. With the right strategies—like turning off unnecessary services, using MFA, restricting access, and securing passwords—I can help reduce the risk of RDP-based attacks. Tomorrow, I’ll explore **RDP logs** generated by our **Windows Server** and create an alert for **RDP Brute Force attacks**.



# Day 16: Creating a Windows Brute Force Alert

## Synopsis
Today’s session focused on **Windows authentication logs** from our server created on **Day 5**. I also demonstrated how to create a **Brute Force alert** for the Windows server using the event code for failed authentication. By the end of this session, I had successfully set up a new alert that will notify us of any potential **RDP brute force** attempts.

## Key Points

1. **Windows Failed Authentication (Event ID 4625)**:
   - **Event ID 4625** documents every failed login attempt on a Windows machine.
   - To query these logs, I used **event.code: 4625** in Elastic’s **Discover** tool, filtering specifically for failed logins.
   - I added fields like **source IP address** and **username** to ensure accurate tracking of failed authentication attempts.

2. **Testing and Validation**:
   - I performed a failed login attempt using **RDP** on my Windows server to verify that the logs was being correctly captured.
   - Successful RDP logins use **Event ID 4624** and are typically associated with **logon type 10** (remote interactive).

3. **Creating the RDP Brute Force Alert**:
   - After verifying that failed RDP logins was being captured, I created a **Brute Force alert** using the **search threshold rule** feature in Elastic.
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
I successfully created a **Brute Force alert** for my Windows server, tracking **Event ID 4625** for failed logins. I also demonstrated how to create more detailed detection rules for improved alerting. Tomorrow, I will create a **dashboard** to visualize where the failed authentication attempts are coming from, providing a more intuitive overview of potential brute force activity. 



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
I successfully created a comprehensive **RDP dashboard** that visualizes **authentication attempts** from the Windows server. Using **Elastic Maps** and **Table visualizations**, the dashboard now offers insights into both **failed** and **successful attempts**, providing a high-level overview of **RDP activity** across different geographical locations. Tomorrow, I’ll dive into a **Command and Control** tactic using the **Mythic framework**, which attackers often use to establish unauthorized sessions.

![rdp dashboard](https://github.com/user-attachments/assets/098bac4e-cdd7-4ec0-9adf-190e77dc86e2)


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
   - **Sliver**: An **open-source C2 framework** that provides similar capabilities to Cobalt Strike. Created by **Bishop Fox**, Sliver supports multiple C2 channels like **HTTP(S), DNS**, and **WwasGuard**.
   - **Mythic**: The **C2 framework** I will use during this challenge. **Mythic** is built with **GoLang, Docker**, and a **Ib-based UI**. It supports multiple **agents** and **C2 profiles** for different communication methods.

4. **Mythic Framework**:
   - Mythic is a versatile C2 framework with a **Ib-based interface** that allows operators to **track payloads and callbacks**.
   - The framework uses **C2 profiles** for agents to manage communication betIen the compromised system and the Mythic server. 
   - Mythic currently supports **21 different agents**, making it a poIrful tool for testing and adversary simulation.

## Recap
I explored the concept of **Command and Control (C2)** and why it's crucial for attackers. I also Int over some of the most common **C2 tools** used by adversaries in the wild, with a focus on **Metasploit, Cobalt Strike, Sliver,** and **Mythic**. Tomorrow, I’ll begin setting up an attack on our **Windows server**, using **Mythic** to deploy an agent and establish a **C2 session**.



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
   - **Goal**: Establish a communication channel betIen the **Windows Server** and the **Mythic C2 server**.

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
- The attacker disables **Windows Defender** to avoid detection, folloId by downloading the **Mythic agent**.
- The **Mythic agent** establishes a **C2 session** with the **Mythic C2 server**.
- Finally, the attacker performs **exfiltration** by downloading the fake `passwords.txt` file.

## Recap
I created an **attack diagram** to visualize the steps of our **C2 operation** using **Mythic**. The diagram outlined the key phases: **initial access**, **discovery**, **defense evasion**, **execution**, **C2 establishment**, and **exfiltration**. Tomorrow, I will begin setting up the **Mythic C2 server** and prepare to execute the attack on our **Windows Server**.



# Day 20: Setting Up Mythic C2 Server

## Synopsis
In this session, I focused on **setting up a Mythic C2 server** using **Vultr** and learned about the basic components of **Mythic**. By the end of the session, you will have deployed your **Mythic instance**, installed necessary packages, configured basic fwaswall settings, and logged into the Mythic **Ib GUI**. I also covered a **high-level overview** of the Mythic interface to understand its key features for **command and control** operations.

## Key Points

1. **Deploying the Mythic Server**
   - Head to **Vultr** to deploy a new server.
   - Choose **Ubuntu** (4GB RAM) and set the **host name** to something identifiable like `mydf-Mythic`.
   - After deployment, log into the server using **SSH**.

2. **Prerequisites Installation**
   - Update and upgrade the server's repository: 
     - `apt-get update && apt-get upgrade -y`
   - Install **Docker Compose** and **Make** (needed for Mythic).

3. **Cloning Mythic Repository**
   - Clone the Mythic repository from GitHub: 
     - `git clone https://github.com/its-a-feature/Mythic`
   - Navigate into the **Mythic dwasctory** and install the necessary Docker containers: 
     - `./install_docker_ubuntu.sh`
   - Start **Mythic** using: 
     - `./mythic-cli start`.

4. **Tightening Fwaswall Settings**
   - Use **Vultr's fwaswall** feature to restrict access to your Mythic server. Only allow connections from trusted **IP addresses** (your own and target machines).
   - Configure **TCP rules** to limit ports and secure access.

5. **Accessing Mythic Ib GUI**
   - Mythic runs on port **7443**. Access the GUI via: 
     - `https://<public IP>:7443`
   - Default credentials: 
     - **Username**: `mythic_admin`
     - **Password**: Located in the `.env` file in the Mythic dwasctory (`cat .env`).

6. **Mythic Ib Interface Overview**
   - **GUI Overview**:
     - **Payloads**: Generate, manage, and track your C2 payloads.
     - **Callbacks**: Track which machines have connected back to your Mythic server.
     - **Artifacts**: Manage files, credentials, and other forensic data.
     - **Reporting**: Generate reports with **MITRE ATT&CK** mappings.
     - **Operations**: Create and manage your campaigns or red team operations.
     - **Customization**: Switch betIen light and dark mode.

## Recap
In this session, I successfully set up the **Mythic C2 server** on **Vultr** and explored the **Ib interface**. I also learned how to **restrict access** to the server using **fwaswall rules** and briefly Int over the main features of Mythic. Tomorrow, I’ll generate a **Mythic agent**, set up the **C2 profile**, and launch an **attack** on the **Windows server** from Day 5.



# Day 21: Performing a Brute Force Attack and Establishing a C2 Connection

## Synopsis
In this session, I performed a full **Brute Force attack** on the **Windows server**, generated a **Mythic agent**, and successfully established a **C2 connection** using the **Mythic C2 framework**. By the end of the session, you will have folloId along in executing **Discovery commands**, evading **Windows Defender**, and exfiltrating a file using the C2 connection.

## Key Points

1. **Setup of the Target Machine (Windows Server)**
   - Created a fake file called `passwords.txt` in **Documents** with a sample password: `Winter2024!`.
   - Changed the **administrator password** to `Winter2024!`.
   - Modified local **password policy** to allow shorter, simpler passwords.

2. **Initial Access - Brute Force Attack**
   - Used **Kali Linux** and the `crowbar` tool to brute force **RDP** access to the Windows server.
   - Added **Winter2024!** to the custom word list to simulate a real-world brute force attack.
   - Successfully authenticated via **RDP** after brute forcing the server with **crowbar**.

3. **Discovery Phase**
   - Ran various **Discovery commands** after gaining access:
     - `whoami`
     - `ipconfig`
     - `net user`
     - `net group`
   - These commands was used to gather basic information on the compromised Windows server.

4. **Defense Evasion**
   - Disabled **Windows Defender** to prevent it from detecting and blocking the **Mythic agent**.

5. **Execution Phase**
   - Created a **Mythic agent** using the **Apollo** agent and the **HTTP** C2 profile.
   - Configured the callback settings to communicate with the **Mythic server** using **HTTP** on port **80**.
   - Used **PowerShell** to download and execute the **Mythic agent** on the Windows server.

6. **Command and Control (C2)**
   - Established a successful **C2 connection** betIen the **Windows server** and the **Mythic C2 server**.
   - Interacted with the **C2 session**, running commands such as `whoami` and `ipconfig` to verify access.

7. **Exfiltration**
   - Used the C2 session to exfiltrate the **passwords.txt** file from the Windows server.
   - Verified that the **password file** contained the stored password `Winter2024!`.

## Recap
In this session, I successfully completed a full attack from **initial access** to **exfiltration** using **Kali Linux** for **Brute Forcing** and **Mythic C2** to establish a **command and control session**. I used **PowerShell** to download the **Mythic agent**, bypassed **Windows Defender**, and extracted sensitive information from the target system.

Next, I will create alerts and dashboards to help detect **Mythic activity** in your environment.

![mythicc2services](https://github.com/user-attachments/assets/a98cd699-e21f-4980-b721-8f0b9cb22bb7)
---
![payload](https://github.com/user-attachments/assets/d430c880-f09b-4bbc-bb3e-4329dd635b96)
---
![C2 established](https://github.com/user-attachments/assets/e76d4a22-2c96-462e-92be-cec7e534b3df)



# Day 22: Creating Alerts and Dashboards for Mythic C2 Detection

## Synopsis
In this session, I focused on creating alerts and dashboards that detect **Mythic C2 activity** generated from **Day 21**. By the end of this session, I will have created a rule to detect **Mythic C2 process creation**, as well as a dashboard to monitor suspicious activity such as **process creation**, **network connections**, and **Microsoft Defender being disabled**.

## Key Points

### 1. **Creating Alerts for Mythic C2**
   - I began by querying **Mythic C2 activity** using **Elastic Search**.
   - Queried for the **service host process** (`servicehost.exe`) and filtered for **process creates (Event ID 1)**.
   - Added filtering based on **SHA-256 hash** and **original file name** (`apollo.exe`).
   - Saved the query and created a **detection rule** for **Mythic C2**:
     - Triggering conditions: **SHA-256 hash** or **original file name (apollo.exe)**.
     - Included additional fields such as **username**, **command line**, and **parent process**.
   - Set the rule to check every 5 minutes with a **critical** severity.

### 2. **Building a Dashboard**
   - Created a dashboard with three panels to visualize **suspicious activity**:
     - **Panel 1: Process Creation (Event ID 1)**
       - Focused on detecting the execution of **PowerShell**, **CMD**, and **rundll32.exe**.
     - **Panel 2: Process-Initiated Network Connections (Event ID 3)**
       - Monitored for processes initiating outbound connections using **Sysmon** logs.
     - **Panel 3: Microsoft Defender Disabled (Event ID 50001)**
       - Detected instances where **Windows Defender** was disabled.

### 3. **Configuring Visualizations**
   - Added necessary fields for each panel, including **source IP**, **destination IP**, **process name**, and **host name**.
   - Excluded noise by filtering out processes such as **Windows Defender**.
   - Saved and organized the dashboard panels for easy access to **suspicious process activity** and **network connections**.

## Recap
In this session, I successfully created detection rules and dashboards to monitor for **Mythic C2 activity**, as well as other suspicious actions such as **process creation**, **network connections**, and **Windows Defender disabling**. These tools will help SOC analysts identify potential threats in their environment. 

Tomorrow, I'll explore setting up a **ticketing system** to help manage alerts and cases efficiently.

![dashboard 2](https://github.com/user-attachments/assets/a47f9bfe-866d-4977-99c8-ebb8bf0dede9)


# Day 23: Introduction to Ticketing Systems with OS Ticket

## Synopsis
In today's session, I covered the importance of **ticketing systems** in tracking alerts, misconfigurations, and ongoing tasks in a SOC environment. I introduced **OS Ticket**, a free and open-source ticketing system, and explored how it can be integrated into a small SOC operation to improve task management, accountability, and auditing.

## Key Points

### 1. **What is a Ticketing System?**
   - A **ticketing system** is a platform for managing and tracking **tasks, alerts, and requests** in an organized manner.
   - Tickets can include:
     - **Security alerts** from monitoring tools.
     - **Customer complaints** or troubleshooting requests.
     - **Audit trails** for actions taken on alerts or incidents.
   - A ticketing system provides an audit trail, which supports **accountability** and **tracking** for actions related to authentication, authorization, and accounting (the **AAA** security model).

### 2. **Popular Ticketing Systems**
   - Common commercial options include:
     - **Jira**
     - **ServiceNow**
     - **Freshdesk**
     - **Zendesk**
   - These platforms allow for **custom fields, ticket routing, task assignments,** and even **SLA (Service Level Agreement)** tracking.

### 3. **OS Ticket: Open Source Ticketing System**
   - **OS Ticket** is a free, open-source ticketing system developed by **Enhancesoft**.
   - **Key Features**:
     - **Customizable ticket fields**: Tailor the ticket system to suit your organization’s needs.
     - **Ticket routing and filters**: Automatically route tickets to the correct team or agent.
     - **Ticket assignment and transfers**: Easily assign tickets and transfer them betIen agents or departments.
     - **SLA Tracking**: Set SLAs to ensure timely responses and resolution of tickets.
   - OS Ticket can be **self-hosted** for free or hosted by OS Ticket for a fee (with additional integrations and support available).

### 4. **Benefits of Using OS Ticket in a SOC Environment**
   - Helps **track security alerts** from tools, ensuring they are addressed systematically.
   - Provides an **audit trail** for accountability.
   - Allows for **custom ticketing workflows**, enabling you to manage alerts and incidents like a real-world SOC team.

## Recap
Today, I explored the concept of **ticketing systems** and introduced **OS Ticket** as a free, open-source tool to help track and manage security alerts. Tomorrow, I will cover how to install and configure **OS Ticket** for use in your SOC environment.

Stay tuned for the practical walkthrough Tomorrow!



# Day 24: Setting Up OS Ticket for SOC Operations

## Synopsis
In today's session, I set up **OS Ticket**, an open-source ticketing system, as part of our SOC environment. This system will help manage and track security alerts, troubleshooting requests, and any incidents in an organized manner.

## Key Points

### 1. **Deploying the OS Ticket Server**
   - **Cloud provider**: I used **Vultr** to deploy a cloud server with **Windows Server 2022** as the OS.
   - **Fwaswall setup**: I configured a fwaswall to restrict access to only authorized users for added security.

### 2. **Installing XAMPP**
   - XAMPP is an easy-to-use Ib server solution for hosting the OS Ticket application.
   - **Steps involved**:
     - Download XAMPP from Apache Friends and install it.
     - Configure **Apache** and **MySQL** within XAMPP.
     - Modify configuration files for **PHPMyAdmin** to allow remote access.
     - Set up fwaswall rules to allow access only to specific ports (80, 443) and secure access.

### 3. **Setting Up the Database for OS Ticket**
   - I used **PHPMyAdmin** to create a **MySQL database** for OS Ticket.
   - **Created a database** named `SOC1-30day-db` and set up user privileges for the database.

### 4. **Installing OS Ticket**
   - **OS Ticket** installation steps:
     - Downloaded the OS Ticket installation files and extracted them to the XAMPP dwasctory.
     - Renamed and configured the sample configuration file (`config.php`).
     - Entered the OS Ticket setup page via the browser and completed the basic installation.
   - **Database Configuration**: Used the database and user credentials I set up earlier to complete the OS Ticket setup.
   - Adjusted **file permissions** for secure operation.

### 5. **Accessing OS Ticket**
   - Successfully accessed the **OS Ticket system** via browser.
   - **Admin Panel**: Demonstrated how to navigate to the admin panel, add new agents, and start configuring the ticketing system for SOC operations.
   - **Agent vs. Client Login**: Differentiated betIen agent login (for managing tickets) and client login (for submitting tickets).

## Recap
Today, I configured a fully functional **OS Ticket system** to manage alerts and tickets in our SOC environment. You now have the capability to use this system to track and manage incidents. Tomorrow, I will integrate OS Ticket with other SOC tools to automate ticket creation for security alerts.

Stay tuned for the next steps on integration!

![osticketsuccess](https://github.com/user-attachments/assets/bcabcdbd-9dda-40b5-8f03-e88f2ddbacd3)

![osticketlogin](https://github.com/user-attachments/assets/261abf6c-8622-4bb6-9ba1-4739583008ce)

![osticketdashboard](https://github.com/user-attachments/assets/30a5efef-e810-4c3d-91f3-69cded424145)



# Day 25: Integrating OS Ticket into Your Tech Stack

## Synopsis
Today, I integrated **OS Ticket** into our **Elastic Stack**, allowing for automatic ticket generation whenever an alert is triggered. This integration is critical for maintaining an organized incident management process in a SOC environment.

## Key Points

### 1. **Setting Up OS Ticket API**
   - Logged into the **OS Ticket** admin panel and navigated to **Manage > API**.
   - Created a new **API Key** for Elastic to use, allowing it to create tickets automatically.
   - Saved the private IP address of our Elastic server to allow internal communications within the **Virtual Private Cloud (VPC)**.

### 2. **Configuring Elastic Stack for OS Ticket Integration**
   - Enabled a **30-day free trial** of Elastic Stack to unlock the ability to use API integrations.
   - Navigated to **Stack Management** in Elastic and created a new connector using the **Ibhook** option.
   - Configured the Ibhook to send data to the OS Ticket API, specifying the **POST request** URL and headers, including the **API key**.

### 3. **Testing the Integration**
   - Encountered a **network connection issue** due to incorrect private IP assignment for OS Ticket.
   - Troubleshooted by manually assigning the correct **private IP** for the OS Ticket server using **Control Panel > Network Settings**.
   - Successfully tested the Ibhook by sending a **POST request** and generating a ticket in OS Ticket.

### 4. **OS Ticket Test Ticket**
   - Verified the successful integration by checking the OS Ticket agent panel and confirming the presence of a newly created ticket, titled with the **challenge subject** (SOC1-30day challenge).
   - This confirmed that OS Ticket was now fully integrated into our SOC environment for ticket tracking.

## Recap
Today, I completed the integration of **OS Ticket** with our **Elastic Stack** to automatically create tickets whenever alerts are triggered. This integration improves tracking, accountability, and auditing, which are essential components of incident management.

### Accomplishments So Far:
1. Deployed Elastic Search and Kibana instances.
2. Installed agents on Windows and Linux servers to push data into Elastic.
3. Created alerts and dashboards for SSH and RDP brute force activity.
4. Identified **C2 activity** using the **Mythic** framework.
5. Installed and configured OS Ticket for incident tracking.
6. Successfully integrated OS Ticket into the SOC environment to automatically generate tickets for alerts.

Tomorrow, I will begin investigating alerts, starting with the **SSH brute force alert**.

Stay tuned for more SOC practices as I dive into alert investigations!

![osticket-test](https://github.com/user-attachments/assets/4ac7ad38-2d0a-40f3-b20c-46fdef13927b)


# Day 26: Investigating an SSH Brute Force Alert

## Synopsis
In today's session, I learned how to investigate an **SSH Brute Force** alert using Elastic and explored some of the key steps in the investigative process. Additionally, I integrated the alerting system into **OS Ticket** to automate ticket creation, enabling efficient tracking of security incidents.

## Key Points

### 1. **Initial Alert Review**
   - Started by reviewing the **SSH Brute Force alert** in Elastic, specifically looking at the source IP, user accounts targeted, and the number of attempts.
   - Discussed the importance of using **timelines** to help in investigations, which provide an organized workspace to investigate alerts.

### 2. **Investigative Steps**
   - Focused on ansIring these key questions during the investigation:
     1. **Is this IP known for brute force activity?**
        - Utilized **AbuseIPDB** and **GreyNoise** to confirm that the IP address had been reported multiple times for malicious activities, including brute force attempts.
     2. **was there any other users targeted by this IP?**
        - Used the **user.name** field in Elastic to identify that the IP had also targeted **root, Oracle, guest,** and **test** accounts.
     3. **was any of the brute force attempts successful?**
        - Queried for **successful logins** by looking for "accepted" login events. No successful logins was found.

   - These steps are part of the **methodology** for investigating brute force alerts, helping you determine whether further investigation or mitigation is requwasd.

### 3. **Alert Closure Process**
   - Since no successful logins was detected, the alert was marked as benign. If successful logins had been detected, the investigation would have continued to track post-login activity.
   - Discussed the importance of following the process and documenting findings in a **ticketing system** like OS Ticket.

### 4. **Modifying Rules for OS Ticket Integration**
   - Int over how to modify Elastic rules to automatically push alerts into OS Ticket.
   - Customized the **alert body** for better context, including variables like the **rule name** and **source IP**.
   - Ensured that the **URL of the triggered alert** was included in the ticket for easy reference, allowing analysts to quickly jump to the alert details in Elastic.

### 5. **Ticketing Workflow**
   - Once the alert is generated in OS Ticket, assigned it to yourself to avoid duplicating effort with other analysts.
   - Documented the investigation and closed the ticket, marking it as resolved.

## Recap
Today, I investigated an SSH brute force alert by:
1. **Reviewing key details** (IP, affected users, success rates).
2. **Cross-referencing external sources** like AbuseIPDB and GreyNoise for further context.
3. **Confirming** whether any logins was successful and checking post-login activity.
4. **Integrating** Elastic alerts with OS Ticket for automatic ticket creation.
5. **Closing** the alert in OS Ticket after determining it was not a threat.

Tomorrow, I will focus on investigating an **RDP brute force alert**, diving deeper into another critical attack vector.



# Day 27: Investigating an RDP Brute Force Alert

## Synopsis
In today's session, I investigated an **RDP Brute Force** alert using the same methodology as the SSH Brute Force investigation. By applying consistent investigative steps, I assessed the impact of the alert, identified key indicators of potential compromise, and evaluated post-login activity to ensure thorough investigation. I also automated ticket creation in **OS Ticket** for better alert management.

## Key Points

### 1. **Initial Alert Review**
   - Investigated the **RDP Brute Force** alert triggered on **August 14th**, noting the source IP address, targeted user (administrator), and the number of brute force attempts (six events).
   - Automated the process of pushing the RDP Brute Force alerts into **OS Ticket** by copying and modifying the settings used in previous alerts for SSH.

### 2. **Key Investigative Steps**
   When investigating RDP brute force attacks, I ask the following questions:

   1. **Is the IP known to perform brute force activity?**
      - **AbuseIPDB** shoId the IP was reported 55 times with an abuse confidence of 48%. The activity was flagged as **RDP brute force** from Russia.
      - **GreyNoise** classified the IP as "internet background noise" due to its **RDP scanning** activity, but not a specific targeted attack.

   2. **Are any other users affected by this IP?**
      - Using Elastic’s **user.name** field, I determined that only the **administrator** account was targeted.

   3. **was any of the brute force attempts successful?**
      - Queried for event code **4624** (successful logins). No successful attempts was detected for this particular IP.

   4. **What activity occurred after the successful login?**
      - In this case, none of the login attempts was successful, so no further activity occurred.

### 3. **Examining a Successful Login**
   - **Example case: Mythic C2** brute-forced attack from **IP 181.215.182.51**.
     - **AbuseIPDB** and **GreyNoise** flagged the IP for nefarious activities (Brute Force, DoS attacks), but it was not widely observed by GreyNoise.
     - The **administrator** account was successfully logged in, which triggered deeper investigation into post-login activity.
     - Used the **logon ID** to track the entwas session and determine what actions took place, including checking for **administrative privileges** and **logoff** events.

### 4. **Key Findings and Workflow**
   - The successful brute force attack led to a short-lived session where the user logged off almost immediately after logging in, indicating likely automated activity. This was likely due to the **crowbar** tool used during the Mythic C2 brute force.

### 5. **Next Steps in Investigation**
   - From the identified **time range** of the successful login, the investigation would shift to:
     - Checking for **process creations**, **persistence mechanisms**, and **command and control (C2)** attempts.
     - Investigating potential **exfiltration** or **lateral movement** during the attacker’s session.

## Recap
I investigated an **RDP Brute Force** alert by:
1. **Assessing the source IP** using AbuseIPDB and GreyNoise to determine the likelihood of malicious intent.
2. **Checking for targeted users** and confirming only the **administrator** account was involved.
3. **Determining if any attempts was successful**, which they was not in this case.
4. In the case of a **successful brute force**, I examined post-login activity, noting **logon ID tracking** as key to following the attacker’s session.

By applying the same structured approach as used in SSH brute force investigations, you can confidently investigate RDP brute force alerts, ensuring thoroughness across different protocols. Tomorrow, I’ll investigate **Mythic C2** agent activity and explore additional malicious behavior.



# Day 28: Investigating Mythic C2 Activity

## Synopsis
Today, I investigated a **Mythic Command and Control (C2)** activity within the environment. I explored how to identify and analyze C2 activity using process creation and network telemetry and investigated the impact of C2 callbacks. The session included advanced techniques for tracking file creations, process GUIDs, and correlating various events to detect malicious behavior.

## Key Points

### 1. **Identifying Mythic C2 Agent**
   - **Initial Query:** Used the known C2 agent name `servicehost dstep rocks.exe` to search for relevant events in **Elastic Discover**.
   - Sorted events by **timestamp** (old to new) to follow the chain of activities.
   - If the agent name is unknown, use **network telemetry** to look for back-and-forth communications or high-volume data transfer.

### 2. **Exploring Suspicious Network Connections**
   - Explored network telemetry using the **process initiated network connections** dashboard, focusing on outbound network connections.
   - Highlighted potential malicious network behavior based on suspicious **file locations** (e.g., executables in `C:\Users\Public\Downloads`) and connections to external IPs over unusual ports (e.g., **port 9999**).
   - Added **destination IP** and **process creation** details to further investigate network connections.

### 3. **Deep Dive into Process GUID**
   - Investigated deeper using **Sysmon telemetry** by analyzing **process GUIDs**. This helped track the exact behavior of the **PowerShell** session.
   - Identified key events such as:
     - **File creation**: `servicehost dstep rocks.exe`.
     - **Process creation**: Execution of the C2 agent.
   - Correlated these events with the **network connections** to map out the attack timeline.

### 4. **Tracking the C2 Agent’s Behavior**
   - Used **process GUID** and **process ID** to explore follow-up activities of the C2 agent.
   - Found **file executable detection** (Sysmon event 29), showing how the agent was loaded.
   - Checked for any child processes spawned by the C2 agent, using **parent process IDs**.

### 5. **Correlating C2 Commands and Endpoint Telemetry**
   - Demonstrated how C2 commands like `netstat` are typically captured at the **network layer**, while commands like `ipconfig` (executed via `cmd.exe`) are captured in **endpoint telemetry**.
   - Correlated **Sysmon process creation events** to match C2 command executions and observed **logon IDs** to track activities.

### 6. **Automating Alerts and OS Ticket Integration**
   - Configured **Elastic Detection Rules** to generate alerts for **Mythic C2** activities.
   - Verified automatic ticket generation in **OS Ticket**, ensuring that all critical C2-related alerts are properly tracked and investigated.

### 7. **Key Tools and Techniques**
   - **Sysmon Process GUID** and **Process ID** correlation for tracking C2 activities.
   - **Network telemetry** to identify suspicious external connections.
   - **File creation and execution analysis** to detect potential payload delivery.
   - Utilized tools like **RITA** for additional C2 detection capabilities (optional).

### 8. **Key Findings and Workflow**
   - Tracked the C2 agent’s initial execution and subsequent network activity.
   - Discovered how C2 frameworks communicate and how to map their behavior to network and endpoint telemetry.

## Recap
I investigated a **Mythic C2** agent by:
1. **Identifying suspicious network connections** through process telemetry.
2. **Tracking file creations and process executions** using **Sysmon event IDs** and **GUIDs**.
3. **Mapping the attack timeline** from **file creation** to **network connection** using **PowerShell** and **process creation events**.
4. **Verifying automatic alerts** in **OS Ticket** for better alert tracking and management.

By following this workflow, I was able to uncover the behavior of the **Mythic C2 agent** and track its communications within the environment. This methodology can be applied to any C2 framework, making it a powerful tool for SOC analysts.

Tomorrow, I will focus on **installing Elastic Defend** for advanced endpoint detection and response (EDR) capabilities.

![osticket-C2-message](https://github.com/user-attachments/assets/78bfed7e-8ac5-444a-88be-70c1f14d4b47)


# Day 29: Installing and Using Elastic Defend (EDR)

## Synopsis
Today, I installed and explored **Elastic Defend**, the endpoint detection and response (EDR) solution offered by **Elastic**. The goal was to learn how to protect endpoints from malicious activity, view detailed telemetry, and automatically respond to threats. This session included hands-on experience with detecting and preventing malware, as well as automating host isolation as part of a response strategy.

## Key Points

### 1. **Installing Elastic Defend**
   - Accessed the **Elastic Defend** integration by navigating to the **Integrations** section in Elastic.
   - Selected **Complete EDR** for full telemetry, including **antivirus, essential EDR**, and **complete EDR** capabilities.
   - Added the integration to the existing **Windows server** using the **Elastic Fleet server** for centralized agent management.
   - Verified the endpoint protection by checking the **Endpoints** section under **Security** in Elastic.

### 2. **Testing Malware Detection with Elastic Defend**
   - Attempted to execute a known malicious file (`SOC1-30.exe`) on the Windows server.
   - **Elastic Defend** successfully blocked and quarantined the file, indicating it detected malware.
   - Observed a **malware alert** and prevention message on the Windows server: "Elastic Security prevented SOC1-30.exe."
   
### 3. **Viewing Malware Telemetry in Elastic**
   - In **Discover**, searched for **malware-related telemetry** generated by **Elastic Defend** within the last 15 minutes.
   - The telemetry revealed:
     - **File details**: file path, quarantine path, file name, and hash.
     - **Alert details**: agent type, process executable, and malicious file event.
   - Examined the **malware prevention alert** under **Security** → **Alerts**, which displayed key details such as the **process tree** showing the relationship betIen **explorer.exe** and **SOC1-30.exe**.

### 4. **Setting Up Automated Response Actions**
   - Edited the **malware prevention rule** to automatically trigger a **host isolation** response whenever a malware event occurs.
   - Configured the **Elastic Defend response action** to isolate the infected Windows server if malware is detected.
   - Verified the automatic isolation of the host after downloading the malicious file again, and tested network connectivity using an infinite ping (`ping 8.8.8.8 -t`).
   
### 5. **Verifying Host Isolation**
   - After triggering the malware detection again, **Elastic Defend** successfully isolated the host, cutting off network access.
   - This demonstrated how Elastic Defend can automatically respond to detected threats and isolate compromised systems for mitigation.

### 6. **Limitations of the Free License**
   - With a **free Elastic license**, users are unable to **remotely isolate hosts**. This feature is available during the **30-day trial** or with a paid subscription.
   - Elastic Defend offers full endpoint protection and monitoring even with the free license, but advanced response capabilities requwas an upgraded plan.

## Recap
In this session, I successfully installed and tested **Elastic Defend**, Elastic’s **EDR solution**. I explored how it detects and prevents malware, investigated telemetry related to malicious file events, and configured automatic response actions to isolate compromised hosts. Key takeaways include the poIr of automated threat response and the visibility Elastic Defend provides into endpoint activity.

With this new EDR capability, you can continue building on your **SOC analyst skwells** by:
1. **Analyzing endpoint telemetry** for various threats.
2. **Creating automated responses** for common attack vectors.
3. **Testing malware detection** by running controlled scenarios and verifying the effectiveness of **Elastic Defend**.

Tomorrow, I will focus on **troubleshooting** as I conclude the 30-day challenge.

