# Automated AWS Infrastructure & GitHub Integration with Ansible

This project automates the provisioning of a production-ready AWS environment using **Ansible**. It follows an industry-standard **Role-Based Architecture** to ensure modularity, scalability, and ease of maintenance.

The playbook performs the following actions:
1.  **Network Setup:** Creates a custom VPC, Public Subnet, Internet Gateway, Route Tables, Network ACLs, and Security Groups.
2.  **Compute Provisioning:** Dynamically fetches the latest Ubuntu 24.04 AMI and launches an EC2 instance.
3.  **CI/CD Integration (Optional):** Automatically uploads the new server's IP and SSH Keys to GitHub Repository Secrets, enabling immediate deployment via GitHub Actions.

## Project Structure

```text
CeylonaAcademy/
├── ansible.cfg                 # Ansible configuration (Inventory path, SSH settings)
├── site.yml                    # Master Playbook (Entry point)
├── inventory/
│   └── local                   # Localhost inventory definition
├── group_vars/
│   └── all.yml                 # Global variables (Region, Project Name)
└── roles/
    ├── aws_network/            # ROLE: Handles VPC, Subnets, Firewalls
    ├── aws_compute/            # ROLE: Handles EC2 Instances & AMIs
    └── github_integration/     # ROLE: Pushes secrets to GitHub (Optional)


## Prerequisites
Ensure you have the following installed on your control node (e.g., WSL2, Ubuntu, or Mac):

Ansible Core (sudo apt install ansible)

Python 3 & Pip

AWS SDK for Python (pip install boto3 botocore)


Ansible Collections:

Bash

ansible-galaxy collection install amazon.aws community.aws community.general

Configuration
1. Global Variables (group_vars/all.yml)
Modify this file to change the region or project name foundation.

YAML

project_name: "CeylonaAcademy"
aws_region: "us-east-1"
2. Local Key Pair
Ensure you have your SSH Private Key (e.g., virginia.pem) stored in the root directory. The github_integration role requires this file to read the key content.

🛠️ Usage
Run the master playbook using the command below. The ansible.cfg file automatically handles inventory paths.

Bash

ansible-playbook site.yml
Interactive Prompts
For security, the playbook will ask for credentials at runtime:

AWS Access Key ID: From your AWS IAM Dashboard.

AWS Secret Access Key: From your AWS IAM Dashboard.

(Optional/Commented) GitHub Token: If running the integration role.

🛡️ Security Notes
Network ACLs: Configured as a stateless firewall allowing standard Web (80/443) and SSH (22) traffic, plus ephemeral ports (1024-65535) for return traffic.

Security Groups: Stateful firewall rules applied to the EC2 instance.

Idempotency: The EC2 task uses exact_count: 1 to prevent duplicate instances from being created on subsequent runs.