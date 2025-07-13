# Terraform + Ansible Infrastructure Automation Using SpaceLift

This project demonstrates end-to-end infrastructure automation using **Terraform** for provisioning and **Ansible** for configuration management. The setup is designed to run in **Spacelift**, where the Terraform stack triggers the Ansible stack using hooks and shared context.

---

## 🚀 Project Overview

* **Provisioning Tool**: Terraform
* **Configuration Management**: Ansible
* **CI/CD Orchestration**: Spacelift

This automation project:

1. Provisions EC2 instances in AWS using Terraform.
2. Uses Spacelift to trigger the Ansible stack once Terraform is complete.
3. Configures each EC2 with required packages using Ansible (e.g., `htop`, `nginx`).
4. Demonstrates secure SSH communication using passwordless login via mounted SSH keys.

---

## 📁 Folder Structure

```bash
project-root/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── ansible/
│   ├── install_htop.yml
│   └── install_nginx.yml
├── id_rsa          # PEM private key used for SSH
├── id_rsa.pub      # Public key copied to EC2s
├── README.md
```

---

## 🧩 How It Works

### Terraform Stack (provision EC2 instances)

* Region: `eu-north-1`
* AMI: Ubuntu-based
* Resources:

  * VPC, Subnets (optional)
  * EC2 Instances with public IPs
  * Key pair name to match SSH key

### Ansible Stack (configure EC2)

* Reads public IPs from Terraform outputs
* Uses mounted `id_rsa` to SSH into EC2s
* Installs:

  * `htop` on all instances
  * `nginx` on a specific subset

---

## 🔐 Authentication & SSH Access

* SSH keypair (`id_rsa`, `id_rsa.pub`) is generated manually.
* `id_rsa` is mounted to Spacelift's Ansible stack.
* Public key is added to EC2s via user data or `ssh-copy-id`.

---

## 🧪 Testing the Setup

To test this entire flow:

1. Trigger the **Terraform stack** from Spacelift.
2. Terraform provisions all infrastructure and outputs IPs.
3. **Spacelift hook** triggers the Ansible stack.
4. Ansible connects to instances using the mounted private key and runs its playbooks.

---

## ✅ Pre-requisites

* Valid AWS credentials configured in Spacelift
* `id_rsa` and `id_rsa.pub` pair
* Required Spacelift Environment variables:

  * `ANSIBLE_PRIVATE_KEY_FILE` → `/mnt/workspace/id_rsa`
  * `ANSIBLE_REMOTE_USER` → `ubuntu`

---

## 📌 Notes

* Ensure EC2 security group allows port 22 (SSH) and 80 (nginx).
* Use Terraform outputs to pass IPs to Ansible dynamically.
* This is ideal for automating dev/test environments quickly.

---

## 📚 Learnings

* Terraform and Ansible can work seamlessly together with the right trigger mechanism.
* SSH key format matters — use PEM (RSA) for compatibility.
* Spacelift simplifies complex multi-step workflows with stack dependencies and secure file mounts.

---

## 📦 Possible Extensions

* Add provisioning for RDS, S3, or VPC networking.
* Use dynamic inventory scripts instead of static `inventory.ini`.
* Monitor instance status using Ansible facts.
* Add CloudWatch or Cost alerts for budgeting.

---

## 🧾 License

MIT License

---

> **Author**: Deepak B
> **Project**: Terraform-Ansible Infra Automation
> **Status**: Functional and extensible
