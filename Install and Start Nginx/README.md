# Ansible NGINX Setup Guide

This repository contains an Ansible playbook (`nginx-playbook.yml`) and an inventory file to install and start NGINX on target instances. Follow these steps to set up two Ubuntu EC2 instances on AWS and use Ansible to configure them with NGINX.

## Prerequisites

1. **Create Two Ubuntu EC2 Instances**: Launch two Ubuntu instances on AWS. These will serve as the control node (server) and the target instance(s).
2. **SSH Access Setup**:
   - Connect to each EC2 instance using SSH from WSL (replace `/path/to/keypair/test.pem` with your key’s actual path and `<IP_ADDRESS>` with the instance's public IP):
     ```bash
     ssh -i /path/to/keypair/test.pem ubuntu@<IP_ADDRESS>
     ```

   - Once connected to the control instance, generate an SSH key pair:
     ```bash
     ssh-keygen
     ```
   - Retrieve the public key:
     ```bash
     cat ~/.ssh/id_rsa.pub
     ```
   - Copy the output.

   - Now, log in to each target instance and set up SSH access:
     - Connect to the target instance:
       ```bash
       ssh -i /path/to/keypair/test.pem ubuntu@<TARGET_INSTANCE_IP>
       ```
     - Generate an SSH key on the target instance:
       ```bash
       ssh-keygen
       ```
     - List files in the `.ssh` directory to confirm:
       ```bash
       ls ~/.ssh
       ```
     - Open the `authorized_keys` file for editing:
       ```bash
       nano ~/.ssh/authorized_keys
       ```
     - Paste the copied public key from the control instance here and save the file.

3. **Install Ansible on the Control Instance**:
   - Update package lists:
     ```bash
     sudo apt update
     ```
   - Install Ansible:
     ```bash
     sudo apt install -y ansible
     ```
   - Verify installation:
     ```bash
     ansible --version
     ```

## Configure the Inventory File

1. Create an inventory file on the control instance:
   ```bash
   nano inventory
   ```
2. Add the IP addresses (public or private IPv4) of the target instances:
   ```plaintext
   [web]
   <TARGET_INSTANCE_IP>
   ```

## Run the Playbook

1. Copy the contents of `nginx-playbook.yml` from this repository.
2. Create the playbook file on your control instance:
   ```bash
   nano nginx-playbook.yml
   ```
3. Paste the playbook contents and save the file.
4. Run the playbook using the command:
   ```bash
   ansible-playbook -i inventory nginx-playbook.yml
   ```

This will install and start NGINX on the target instances. You can verify NGINX is running by navigating to the target instance's IP address in a web browser.

---
