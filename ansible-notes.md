
# Ansible Notes

These are my curated DevOps notes on **Ansible** covering setup, ad-hoc commands, playbooks, roles, and real-world examples.

---

## 🔐 Passwordless SSH Authentication

To let Ansible access other servers without prompting for a password:

1. Generate SSH key pair on the Ansible control node:
   ```bash
   ssh-keygen
   ```

2. Copy the **public key** (`id_rsa.pub`) to each target machine’s `authorized_keys`:
   ```bash
   ssh-copy-id user@target-server
   ```

3. Test SSH access:
   ```bash
   ssh user@target-server
   ```

Now Ansible can communicate with target nodes without passwords.

---

## 📁 Ansible Inventory

The inventory file contains IP addresses or hostnames of target servers.

Example:
```ini
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20
```

---

## ⚡ Ad-Hoc Commands

Quick, one-time commands using Ansible without writing a playbook:

```bash
ansible -i inventory all -m shell -a "touch devopsclass"
```

Use ad-hoc for 1–2 commands. For more complex tasks, use playbooks.

---

## 📘 Playbooks

Used to automate multiple steps using YAML syntax.

### Example: Install and start Nginx

```yaml
- name: Install and start nginx
  hosts: all
  become: true

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
```

Run with verbosity:
```bash
ansible-playbook -vvv -i inventory nginx-playbook.yaml
```

---

## 🧱 Roles

For reusable and organized playbooks.

Create a new role:
```bash
ansible-galaxy role init kubernetes
```

### Role Structure:
```
kubernetes/
├── README.md       # Overview of the role
├── defaults/       # Default variables
├── files/          # Static files to be copied
├── handlers/       # Handlers like service restart
├── meta/           # Role dependencies and metadata
├── tasks/          # Main tasks
├── templates/      # Jinja2 templates
├── tests/          # Sample test playbook
└── vars/           # Variables with higher precedence
```

---

## 🧠 Pro Tip: Git Branching in DevOps Context

- Use **feature branches** for developing new features (e.g., Uber Bikes, Uber Intercity).
- After testing, merge into **main branch**.
- Create a **release branch** for final testing before customer delivery.
- **Hotfixes** should go into both main and release branches.
- The **main branch** should always be up-to-date.
- The **release branch** is only for stable production-ready code.

---

_These notes are part of my ongoing DevOps learning. Stay tuned for more updates._
