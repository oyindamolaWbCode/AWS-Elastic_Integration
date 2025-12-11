# AWS-Elastic_Integration
The Deployment of Elastic SIEM integrated with AWS EC2 infrastructure for centralized log collection, monitoring, and security analysis. The goal was to simulate a real-world cloud security monitoring setup using Elastic Agent and Fleet to ingest host-level telemetry from AWS workloads.

#Architecture

Cloud Provider: AWS EC2

OS: Amazon Linux 2023

Elastic Stack: Elastic Security (Kibana + Fleet + Elasticsearch)

Agent: Elastic Agent v9.2.2

Instance type used for lab: t3.micro (small practice instance)
<img width="1366" height="630" alt="Agent-listener" src="https://github.com/user-attachments/assets/dc5a3263-5967-4def-aa74-1a40afb3bc26" />

1) Elastic Fleet — Agent listed as Healthy

<img width="1366" height="630" alt="Agent-listener" src="https://github.com/user-attachments/assets/dc5a3263-5967-4def-aa74-1a40afb3bc26" />
 Caption: Elastic Fleet showing a healthy agent (host ip-172-31-43-183.eu-north-1.compute.internal) with applied agent policy and telemetry metrics.

2) Elastic Agent install & enrollment output
<img width="1366" height="461" alt="AWS Install" src="https://github.com/user-attachments/assets/01af2570-f811-4bba-89e2-a1af9f854216" />

Caption: Terminal output confirming the Elastic Agent installation and successful enrollment to Fleet. Note the log lines: "Waiting For Enroll...", "Successfully enrolled the Elastic Agent", and service started messages.

3) EC2 AMI selection for the lab
4) <img width="1366" height="612" alt="AWS snapshot" src="https://github.com/user-attachments/assets/85e9e69f-6fa7-4fe9-9444-c1ee352b8e74" />
Caption: AWS console showing the Amazon Linux 2023 AMI selected for the EC2 instance used in the lab.

#Steps performed 

1. Provision EC2 host

Launch an EC2 instance using Amazon Linux 2023 (AMI shown in screenshot).

Configure security group to allow SSH (port 22) and allow outbound HTTPS (port 443) for agent enrollment and communication with Elastic Cloud.

2. Prepare the host

SSH into the instance as ec2-user.

Update packages: sudo yum update -y (or sudo dnf update -y depending on the AMI package manager).

3. Download & install Elastic Agent

Download the appropriate Elastic Agent tarball for linux x86_64.

Extract and run the installer as root.

Example commands 

sudo mkdir -p /opt/Elastic/Agent
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-9.2.2-linux-x86_64.tar.gz
sudo tar xzf elastic-agent-9.2.2-linux-x86_64.tar.gz -C /opt/Elastic/Agent --strip-components=1
cd /opt/Elastic/Agent
sudo ./elastic-agent install

4. Enroll to Fleet

Generate an enrollment token in Kibana (Fleet -> Enroll new agent).

Use the enrollment token during the agent install or enroll command.

Confirm logs show successful enrollment (see AWS Install.png).

5. Validate agent in Kibana Fleet

Open Kibana > Fleet > Agents.

Confirm the new agent is listed and shows Healthy, the assigned policy, and recent activity (see Agent-listener.png).

#Observations & lessons learned

Telemetry pipeline validation is critical. The lab highlights how missing connectivity or misconfigured policies can create blind spots in monitoring.

Agent lifecycle: Installing, enrolling, and restarting the agent are common steps you will repeat in production and labs; logs during these steps are the primary source of truth when debugging enrollment issues.

Cloud security groups: Ensure outbound HTTPS is allowed so the agent can reach Elastic Cloud or the Fleet server; otherwise enrollment will fail.
