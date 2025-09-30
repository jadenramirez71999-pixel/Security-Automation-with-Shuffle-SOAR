# Security-Automation-with-Shuffle-SOAR

### Objective

As I continued my journey into cybersecurity, I wanted to move beyond theory and build a practical project that demonstrates real-world skills. My goal with this project, **SOAR-Flow**, was to integrate a **SOAR platform** with a **SIEM** to automate key tasks in the incident response lifecycle. This hands-on experience was a deep dive into the practical application of automation, designed to showcase my ability to build and manage a workflow from the ground up.

---

### Skills Developed

* **SOAR Implementation:** I gained hands-on experience deploying and configuring Shuffle, an open-source SOAR solution.
* **API Integration:** I learned how to connect different platforms (Wazuh, TheHive, VirusTotal) by leveraging their APIs to create a cohesive workflow.
* **Threat Intelligence:** I built a system to automatically enrich alerts with threat intelligence from external sources, which is a critical skill for any security analyst.
* **Log Analysis:** This project strengthened my ability to interpret and act on logs from a SIEM, understanding what security events are most important to respond to.
* **Workflow Automation:** I designed and implemented a logical automation workflow that reduces manual effort and speeds up response time in a SOC environment.

---

### Tools of the Trade

For this project, I used a powerful combination of open-source tools:

| Tool | Description |
|---|---|
| **Wazuh SIEM** | My core platform for threat detection and log management. |
| **TheHive** | A security incident response platform for case management. |
| **Shuffle** | An open-source SOAR platform that serves as the "glue" for my workflow. |
| **VirusTotal API** | A key external source for malware and URL reputation checks. |
| **AbuseIPDB API** | Used to check if a specific IP address has a history of malicious activity. |
| **Discord Webhook** | A simple way to send real-time alerts to my team or myself for instant notifications. |

---

### My Lab Setup & Walkthrough

The project was built on two virtual machines running Ubuntu 24.04 LTS to keep everything isolated and organized.

### Building the Foundation

My first challenge was setting up the two core platforms: Wazuh and TheHive. I followed their official installation guides carefully, making sure to handle dependencies and configure them correctly to communicate with each other.

* **Wazuh SIEM:**
    I began with a fresh Ubuntu VM and used their one-line installer to get Wazuh up and running.

    ```bash
    curl -sO [https://packages.wazuh.com/4.10/wazuh-install.sh](https://packages.wazuh.com/4.10/wazuh-install.sh) && sudo bash ./wazuh-install.sh -a
    ```

    After the installation, I extracted the credentials and was able to access the dashboard to confirm everything was working.

    ![Wazuh Dashboard](https://github.com/user-attachments/assets/1ab85349-4e1f-4916-a363-ed0a1c8030c6)

* **TheHive:**
    Next, I installed TheHive, which required me to configure several dependencies like Java, Cassandra, and ElasticSearch. The default credentials were easy to remember and allowed me to quickly start a new case to test the system.

    ```bash
    # Install dependencies
    apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release

    # Install Java, Cassandra, and ElasticSearch (commands omitted for brevity)

    # Install TheHive
    wget -O- [https://archives.strangebee.com/keys/strangebee.gpg](https://archives.strangebee.com/keys/strangebee.gpg) | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
    echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] [https://deb.strangebee.com](https://deb.strangebee.com) thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
    sudo apt-get update
    sudo apt-get install -y thehive
    ```

    ![TheHive Dashboard](https://github.com/user-attachments/assets/22705378-3dae-449f-926c-0c25109a6a40)

* **Shuffle SOAR:**
    On a separate, dedicated VM, I used Docker to get Shuffle up and running. This was a straightforward process and provided a clean, isolated environment for my automation engine.

    ```bash
    # Clone the Shuffle repository
    git clone [https://github.com/Shuffle/Shuffle.git](https://github.com/Shuffle/Shuffle.git)
    cd Shuffle

    # Build and run Shuffle with Docker Compose
    sudo docker-compose up -d
    ```

    ![Shuffle Dashboard](https://github.com/user-attachments/assets/287a3125-11ba-48d7-be7b-f0e7a35890df)

---

### Automating Incident Response

With my tools installed, the real fun began. I designed a workflow in Shuffle that would automate key steps in my incident response.

1.  **Trigger:** I configured Wazuh to send alerts to a webhook in Shuffle whenever a suspicious event was detected, acting as the starting point for my workflow.
2.  **Enrichment:** Once an alert was received, my playbook automatically queried VirusTotal and AbuseIPDB to gather threat intelligence on any suspicious IPs or file hashes. This is a critical step that saves a security analyst from having to perform these manual lookups.
3.  **Case Creation:** If a threat was confirmed, the workflow used TheHive's API to automatically create a new case. This ensured that every confirmed incident was tracked and logged for further analysis.
4.  **Notification:** I integrated a Discord webhook to send real-time notifications, so my team would be instantly aware of new, high-priority incidents without having to constantly monitor a dashboard.

### Final Thoughts & Next Steps

This project was a fantastic exercise in connecting different security tools and automating a complex workflow. It showed me how much more efficient a SOC can be with the right orchestration.

The next steps for this project include:
* Adding an auto-mitigation step to automatically block high-risk IPs at the firewall level.
* Integrating with other threat intelligence feeds to get a more comprehensive view of a threat.
* Developing playbooks for different types of incidents, such as malware or data exfiltration attempts.

This project is a testament to the power of open-source tools and my ability to use them to solve real-world problems.
