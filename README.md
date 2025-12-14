# Automated AWS Infrastructure & GitHub Integration with Ansible

This project automates the provisioning of a production-ready AWS environment using **Ansible**. It follows an industry-standard **Role-Based Architecture** for modularity, scalability, and maintainability.

---

## 📁 Project Structure

```text
ExampleProject/
├── ansible.cfg                 # Ansible configuration (inventory, SSH, output)
├── site.yml                    # Master Playbook (entry point)
├── inventory/
│   └── local                   # Localhost inventory definition
├── group_vars/
│   └── all.yml                 # Global variables (region, project name)
└── roles/
    ├── aws_network/            # VPC, Subnets, Firewalls
    │   ├── defaults/
    │   │   └── main.yml
    │   └── tasks/
    │       └── main.yml
    └── aws_compute/            # EC2 Instances & AMIs
        ├── defaults/
        │   └── main.yml
        └── tasks/
            └── main.yml

```

---

## 🚀 Features

- **Network Setup:** Custom VPC, public subnet, Internet Gateway, route tables, NACLs, and security groups.
- **Compute Provisioning:** Dynamically fetches the latest Ubuntu 24.04 AMI and launches an EC2 instance.
- **CI/CD Integration (Optional):** Uploads server IP and SSH keys to GitHub Repository Secrets for seamless GitHub Actions deployment.

---

## 🛠️ Prerequisites

- **Ansible Core:**  
  `sudo apt install ansible`
- **Python 3 & pip**
- **AWS SDK for Python:**  
  `pip install boto3 botocore`
- **Ansible Collections:**  
  `ansible-galaxy collection install amazon.aws community.aws community.general`

---

## ⚙️ Configuration

**Global Variables:**  
   Edit `group_vars/all.yml` to set your project name and AWS region:
   ```yaml
   project_name: "ExampleProject"
   aws_region: "us-east-1"
   ```
---

## ▶️ Usage

Run the master playbook:
```bash
ansible-playbook site.yml
```
You will be prompted for:
- **AWS Access Key ID**
- **AWS Secret Access Key**

---

## 🔒 Security Notes

- **Network ACLs:** Stateless firewall allows web (80/443), SSH (22), and ephemeral ports (1024-65535).
- **Security Groups:** Stateful firewall for EC2.
- **Idempotency:** EC2 creation uses `exact_count: 1` to avoid duplicates.

---

## 🏗️ Architecture Overview

### 1. `ansible.cfg`
- Sets default inventory, disables host key checking, and enables YAML output.

### 2. `site.yml`
- Orchestrates prompts and roles: network → compute → (optional) GitHub integration.

### 3. `aws_network` Role
- Builds VPC, subnet, IGW, route tables, NACLs (including ephemeral ports), and security groups.

### 4. `aws_compute` Role
- Looks up latest Ubuntu AMI dynamically.
- Launches a `t3.micro` EC2 instance with idempotency.

---

## 🏃 Quickstart

1. **Get AWS Access Keys:**  
   - AWS Console → IAM → Users → Security Credentials → Create Access Key

2. **Run the Playbook:**  
   ```bash
   ansible-playbook site.yml
   ```
   Enter credentials when prompted.

3. **Result:**  
   Infrastructure is provisioned and ready for CI/CD deployment.

---

## 📚 Further Reading

See the included Medium article draft for a detailed breakdown of each file and architectural decision.

