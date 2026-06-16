# Ansible Custom Script Assignment

## Project Overview

This project demonstrates how to use **Ansible** to copy and execute a custom shell script on multiple remote hosts.

## Assignment

1. Create a shell script that adds the text:

```text
This text has been added by custom script
```

to the file:

```text
/tmp/1.txt
```

2. Execute the script on **all managed hosts** using an Ansible Playbook.

---

# Architecture

```text
                    +----------------------+
                    |      Local PC        |
                    +----------+-----------+
                               |
                               | SSH
                               |
                    +----------v-----------+
                    |    Master Node       |
                    | (Ansible Control)    |
                    +----------+-----------+
                               |
               -----------------------------------
               |                                 |
               | SSH                             | SSH
               |                                 |
       +-------v--------+                +--------v--------+
       |    Slave1      |                |    Slave2       |
       | Ubuntu EC2     |                | Ubuntu EC2      |
       +----------------+                +-----------------+
```

---

# Prerequisites

* AWS EC2 Ubuntu Instances
* Ansible installed on the Master Node
* Passwordless SSH configured between Master and Slaves
* SSH Port (22) enabled in the Security Group

---

# Project Structure

```text
ansible-script-assignment/
│
├── inventory
├── ansible.cfg
├── README.md
│
├── scripts/
│   └── custom_script.sh
│
└── playbooks/
    └── run_script.yml
```

---

# Inventory File

```ini
[all]
slave1 ansible_host=172.31.45.233
slave2 ansible_host=172.31.40.165

[all:vars]
ansible_user=ubuntu
```

> Replace the IP addresses with your own private IPs if they are different.

---

# Ansible Configuration

**ansible.cfg**

```ini
[defaults]
inventory=inventory
host_key_checking=False
```

---

# Shell Script

**scripts/custom_script.sh**

```bash
#!/bin/bash

echo "This text has been added by custom script" >> /tmp/1.txt
```

---

# Ansible Playbook

**playbooks/run_script.yml**

```yaml
---
- name: Copy and Run Custom Script on All Hosts
  hosts: all
  become: yes

  tasks:

    - name: Copy script to remote servers
      copy:
        src: ../scripts/custom_script.sh
        dest: /tmp/custom_script.sh
        mode: '0755'

    - name: Execute the script
      command: /tmp/custom_script.sh
```

---

# Verify Ansible Connectivity

```bash
ansible all -m ping
```

Expected Output

```text
slave1 | SUCCESS
slave2 | SUCCESS
```

---

# Execute the Playbook

```bash
ansible-playbook playbooks/run_script.yml
```

---

# Verify the Result

Using Ansible:

```bash
ansible all -a "cat /tmp/1.txt"
```

Expected Output

```text
This text has been added by custom script
```

Or verify individually:

```bash
ssh slave1
cat /tmp/1.txt
```

```bash
ssh slave2
cat /tmp/1.txt
```

---

# Technologies Used

* AWS EC2
* Ubuntu Linux
* Ansible
* Shell Scripting (Bash)
* SSH
* Git
* GitHub

---

# Learning Outcomes

This project helped me understand:

* Setting up an Ansible Control Node
* Configuring passwordless SSH authentication
* Creating an Ansible Inventory
* Writing Ansible Playbooks
* Using the **copy** module to transfer files
* Executing shell scripts remotely using Ansible
* Verifying remote execution
* Managing projects with Git and GitHub

---

# Author

**Rasiya Saju**

GitHub: https://github.com/rasiyasaju
