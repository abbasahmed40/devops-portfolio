# DevOps Portfolio: EC2 Instance Configuration with Ansible

This project automates the configuration and management of an **AWS EC2 instance** using **Ansible**. It includes tasks like configuring SSH security, setting up a domain with **AWS Route 53**, and automating SSL certificate generation with **Certbot**. **Ansible Vault** is used to securely store sensitive AWS credentials.

---

## Overview

This project includes the automation of several key tasks using **Ansible**. Below is an overview of the tasks and the corresponding roles created for each.

## Key Tasks and Ansible Roles

1. **EC2 Instance Setup**:
   - Launched an **EC2 instance** on AWS.
   - Configured **security groups** to initially allow SSH access on **port 22**, and later changed to **port 44422** for added security.

2. **Automated Configuration with Ansible Roles**:
   - Created and organized Ansible roles for each configuration task:
     - **`hostname`**: Sets the hostname of the EC2 instance.
     - **`security`**: Configures SSH settings for enhanced security (disabling password authentication and enabling key-based authentication).
     - **`certbot-route53`**: Installs Certbot and generates a **wildcard SSL certificate** for `test-p.xyz` using DNS validation with **Route 53**.

3. **AWS Credentials Security with Ansible Vault**:
   - Used **Ansible Vault** to securely encrypt AWS credentials (`aws_access_key_id` and `aws_secret_access_key`), avoiding sensitive information in plaintext.

4. **DNS Management with AWS Route 53**:
   - Configured a **Hosted Zone** in **AWS Route 53** for the domain `test-p.xyz`.
   - Set up **DNS records** to point the domain to the EC2 instance’s static public IP.

---
## Certbot Policy for Route 53 DNS Validation

To enable Certbot to use DNS validation with Route 53, a specific IAM policy is required. This policy grants permissions to list hosted zones, manage record sets, and retrieve change statuses. 

**Certbot Policy Example** (Replace `YOURHOSTEDZONEID` with your actual Route 53 Hosted Zone ID):

```json
{
    "Version": "2012-10-17",
    "Id": "certbot-dns-route53-sample-policy",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:ListHostedZones",
                "route53:GetChange"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "route53:ChangeResourceRecordSets"
            ],
            "Resource": [
                "arn:aws:route53:::hostedzone/YOURHOSTEDZONEID"
            ]
        }
    ]
}
```
## Ansible Vault for Secure AWS Credentials

To securely manage sensitive AWS credentials, **Ansible Vault** is used to encrypt variables directly. Here’s a step-by-step guide to the Ansible Vault setup in this project.

### Setting Up Ansible Vault

1. **Configure `vault_password_file`**:

   First, we set up a `vault_password_file` for Ansible to use automatically when decrypting vault-protected variables. This file is specified in the **`ansible.cfg`**:

   ```
   [defaults]
   vault_password_file = .vault_password
    
   ```
The .vault_password file contains the password for Ansible Vault and should be securely stored and ignored by Git (included in **.gitignore**).

Each key was encrypted individually using the ansible-vault encrypt_string command as shown below:


```

ansible-vault encrypt_string  --name 'certbot_aws_secret_access_key'

```
than enter for key and than ctrl+d twice to gernerate encrypted keys.
