# DevOps Portfolio: AWS EC2 & Ansible Automation

## Overview
This project automates the configuration of an AWS EC2 instance using **Ansible**. It includes tasks like setting up the server’s hostname, installing necessary packages, securing AWS credentials with **Ansible Vault**, configuring SSH security, and obtaining a wildcard SSL certificate with **Certbot** using **AWS Route 53** for DNS management.



---

## Prerequisites

Before running this project, ensure you have the following:

- **A domain name**: I used `test-p.xyz` bought from **NameSilo**.
- **AWS Account**: Access to create and manage EC2 instances and Route 53 hosted zones.
- **Ansible installed**: Ensure that Ansible is installed and set up on your local machine or server.
- **IAM Access**: Permissions to create EC2 instances, configure Route 53, and use Certbot.

---
## Tasks Completed

This project includes the following key tasks:

1. **EC2 Instance Setup**:
   - Launched an **EC2 instance** using the AWS EC2 service.
   - Configured **security groups** to initially allow SSH access on **port 22**.
   - Later opened **port 44422** for SSH access to enhance security and reduce exposure of the default port.
   - Set the **hostname** of the EC2 instance to match the domain `test-p.xyz`.

2. **Automated Configuration with Ansible**:
   - Created Ansible roles to configure the EC2 instance automatically:
     - **`hostname_config`**: Used Ansible to set the hostname of the EC2 instance.
     - **`ssh_security`**: Configured SSH settings for enhanced security (disabling password authentication and enabling key-based authentication).
     - **`certbot`**: Automated the installation of **Certbot** and generation of a **wildcard SSL certificate** for the domain `test-p.xyz` using DNS validation with **Route 53**.

3. **AWS Credentials Security with Ansible Vault**:
   - Used **Ansible Vault** to securely store AWS credentials (`aws_access_key_id` and `aws_secret_access_key`) to avoid exposing sensitive information in the code.

4. **DNS Management with AWS Route 53**:
   - Configured a **Hosted Zone** in **AWS Route 53** for the domain `test-p.xyz`.
   - Set up **DNS records** in Route 53 to point the domain to the EC2 instance’s Elastic public IP address.

---

## Code Snippets

Here are key snippets from the project to illustrate the tasks:

### 1. Setting Up AWS Credentials with Ansible Vault

To secure AWS access keys, I used Ansible Vault:

```bash
ansible-vault create vault/aws_credentials.yml


