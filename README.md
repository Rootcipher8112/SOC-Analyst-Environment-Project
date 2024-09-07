# SOC Analyst Environment Project

## Project Overview

This 30 day project, when completed, is designed to simulate a real-world **Security Operations Center (SOC) environment**, providing a practical demonstration of the key techniques, theories, and practices used in modern cybersecurity operations. By setting up and managing an ELK stack (Elasticsearch, Logstash, and Kibana), deploying various servers, and configuring centralized monitoring through tools like Elastic Agent and Fleet Server, this lab environment mimics the projects and workflows encountered by SOC analysts. The intent over the 30 days is to showcase the ability to manage security data across multiple endpoints, respond to security events, and maintain a secure network infrastructure.

This comprehensive environment not only highlights my technical proficiency in deploying and managing critical security tools but also demonstrates a deep understanding of cybersecurity principles. Through the project, I aim to demonstrate my hands-on experience with log management, system monitoring, and centralized policy enforcement, skills that are essential for ensuring the protection and resilience of enterprise-level IT infrastructures.

# Day 1: Logical Diagram Setup for SOC Analyst Environment

## Introduction
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

# Day 2: Introduction to the ELK Stack

## Introduction
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

## Introduction
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

## Introduction
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

## Introduction
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

## Introduction
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

## Introduction
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





