# Ansible Automation Project

## Overview

This project demonstrates the setup and configuration of an Ansible environment. The project includes setting up an Ansible controller and four client nodes, verifying connectivity, and deploying configurations using an Ansible playbook. The playbook is designed to perform three main tasks across the client nodes.

## Project Structure (See attached diagram in the screenshoot)

1. **Setup Ansible Controller and Clients**
   - Ansible Controller: Ubuntu
   - Four Clients: 2 Amazon Linux and 2 Ubuntu

2. **Verify Setup with Ad-hoc Command**
   - Use the `ping` module to verify connectivity between the Ansible controller and the client nodes.

3. **Ansible Playbook**
   - The playbook consists of three plays:
     - Play 1: Deploy Apache Web Server on Ubuntu clients with a custom HTML page.
     - Play 2: Install Git on all Ubuntu clients.
     - Play 3: Display a debug message with the developer's name on all client nodes.

## Setup Instructions

### 1. Configure Ansible Controller

After launching the EC2 instance, ensure SSH (port 22) is granted in the EC2 security group. Use CLI to connect to the EC2 instance via SSH.

#### Rename the Controller

```bash
sudo hostnamectl set-hostname name-controller
exit
```

Reconnect to the controller:

```bash
ssh -i path/to/your/key.pem ubuntu@controller_ip
```

#### Switch to Root User

```bash
sudo -i
```

#### Check and Install Python3

Verify if Python3 is installed:

```bash
python3 --version
```

If Python3 is not installed, run:

```bash
sudo apt install python3
```

#### Install Ansible

Update and install Ansible:

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Verify the Ansible installation:

```bash
ansible --version
```

### 2. Configure SSH Access

Generate an SSH key pair on the controller:

```bash
ssh-keygen -t rsa
```

Copy the public key to each client node. Replace `client_user` and `client_ip` with the appropriate username and IP address of each client:

```bash
ssh-copy-id client_user@client_ip
```

### 3. Configure the Ansible Inventory

Edit the `/etc/ansible/hosts` file to include your client nodes. For example:

```ini
[ubuntu_hosts]
ubuntu_client_1 _IP_ADDRESS_1 
ubuntu_client_2_IP_ADDRESS_2 

[Amazon_Linux_hosts]
amazon_client_1 ansible_host_IP_ADDRESS_3 
amazon_client_2 ansible_host_IP_ADDRESS_4 
```

### 4. Verify Setup

Use the `ping` module to verify connectivity between the Ansible controller and the client nodes:

```bash
ansible all -m ping
```

### 5. Run the Playbook (See attached the playbook file and the outputs in screenshot attached)

Execute the playbook using the following command. Ensure you have the playbook (`Ansible_Apache_playbook.yml`), variables file (`variables.yml`), and the custom HTML file (`index.html`) in your working directory:

```bash
ansible-playbook Ansible_Apache_playbook.yml
```

### 6. Check Results

- **Apache Web Server Deployment:**
  Verify that Apache is installed and running on Ubuntu clients by accessing the custom HTML page in a web browser. Use the IP address of the Ubuntu clients to check the page.

- **Git Installation:**
  Verify that Git is installed on the Ubuntu clients by SSHing into the clients and running:

  ```bash
  git --version
  ```

- **Debug Message:**
  Verify that the debug message is displayed on all client nodes by reviewing the playbook output.

## Summary

This setup ensures automated deployment and configuration management across multiple servers using Ansible, enhancing efficiency and consistency in managing your infrastructure. By following the steps outlined above, you can quickly set up and verify an Ansible environment and deploy configurations across various client nodes.
