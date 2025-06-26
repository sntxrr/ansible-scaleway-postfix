# Postfix SMTP Relay with Ansible & 1Password

Easily configure a secure, production-ready Postfix SMTP relay on any server using Ansible, with secrets managed via [1Password CLI](https://developer.1password.com/docs/cli/). This project is designed for public sharing, learning, and blogging—no hardcoded secrets, no vendor lock-in, just best practices.

---

## 🚀 Features
- **Automated Postfix SMTP relay setup** on any Linux host
- **Secrets pulled securely from 1Password** (no plaintext credentials)
- **Idempotent, repeatable, and auditable** via Ansible
- **Customizable for any SMTP provider or relay scenario**
- **Beginner-friendly**: clear structure, comments, and error handling

---

## 📦 Prerequisites
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (v2.9+ recommended)
- [1Password CLI (`op`)](https://developer.1password.com/docs/cli/get-started/)
- A 1Password account with a vault containing your SMTP credentials
- Access to a Linux server (cloud VM, VPS, etc.)

---

## 🛠️ Setup Instructions

1. **Clone this repository:**
   ```sh
   git clone https://github.com/yourusername/ansible-scaleway-postfix.git
   cd ansible-scaleway-postfix
   ```

2. **Configure your 1Password CLI session:**
   - Sign in to your 1Password account:
     ```sh
     op signin --account your-1password.1password.com
     ```
   - Ensure your SMTP credentials are stored in a vault (default: `Private`).

3. **Review and customize variables in `postfix-standalone.yml`:**
   - `op_account_name`: Your 1Password account name (default: `your-1password`)
   - `op_vault_name`: The vault containing your SMTP credentials
   - `op_postfix_item`: The item name for your SMTP credentials (e.g., `Scaleway`)

4. **Run the playbook:**
   ```sh
   ansible-playbook -i inventory postfix-standalone.yml
   ```
   By default, this uses the provided `inventory` file to specify your target hosts.

---

## 🗂️ Example Inventory File

The `inventory` file defines which servers Ansible will target. Here's an example:

```ini
ec2instance1 ansible_host=ec2instance1 ansible_user=root
ec2instance2 ansible_host=ec2instance2 ansible_user=root
#ec2instance3 ansible_host=ec2instance3.your-domain.com ansible_user=yourusername ansible_ssh_private_key_file=~/.ssh/id_ed25519
```

- Each line represents a host.
- `ansible_host`: The hostname or IP address of the server.
- `ansible_user`: The SSH user Ansible will use to connect.
- `ansible_ssh_private_key_file`: (Optional) Path to your SSH private key for authentication.
- Lines starting with `#` are comments and ignored by Ansible.

**Tip:**
- You can add as many hosts as you like, or group them using Ansible inventory groups for more advanced scenarios.
- For more, see the [Ansible inventory documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html).

---

## 📋 Example Usage

```yaml
# postfix-standalone.yml
- hosts: all
  become: yes
  vars:
    op_account_name: your-1password
    op_vault_name: Private
    op_postfix_item: Scaleway
  roles:
    - postfix
```

---

## 🔒 Security Notes
- **No secrets are stored in this repo.** All credentials are pulled at runtime from 1Password.
- **Never commit your inventory files or secrets to a public repo.**
- The playbook will prompt you to sign in to 1Password if your session is not active.

---

## ⚙️ Customization
- **Change SMTP provider:** Just update your 1Password item and playbook variables.
- **Add more hosts:** Use your own inventory file to target multiple servers.
- **Extend the role:** Add DKIM, SPF, or monitoring as needed.

---

## 📝 Contributing & Blogging
- PRs, issues, and suggestions are welcome!
- If you blog about this project, please link back to the repo.
- For questions or improvements, open an issue or submit a pull request.

---

## 📚 Resources
- [Ansible Documentation](https://docs.ansible.com/)
- [1Password CLI Docs](https://developer.1password.com/docs/cli/)
- [Postfix Documentation](http://www.postfix.org/documentation.html)

---

**Happy automating!** 