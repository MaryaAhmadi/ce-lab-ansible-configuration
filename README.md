# Lab M5.07 - Configuration Management with Ansible

**Cloud Engineering Bootcamp - Module 5: Cloud Automation & CI/CD**

This project demonstrates automated server configuration using **Ansible** on a local macOS machine (for classroom/testing purposes). It installs and configures **nginx** via Homebrew, deploys a custom index page using Jinja2 templates, creates necessary directories, and sets up a basic web server. All configurations are idempotent and tested locally.

A GitHub Actions workflow is included to lint and dry-run the playbooks on every push/PR.

## Project Overview

Ansible playbooks that:
- Install required packages (nginx, git, curl)
- Create application directories
- Deploy Jinja2-templated configurations and a custom index.html
- Configure a reusable nginx web server role
- Validate everything via GitHub Actions CI

**Tested Environment**: macOS (Apple Silicon) with Homebrew-installed nginx

## Project Structure
.
├── ansible.cfg                  # Ansible configuration defaults
├── inventory.ini                # Local inventory (localhost)
├── group_vars/
│   └── all.yml                  # Shared variables (app_name, environment, etc.)
├── templates/
│   └── app.conf.j2              # Example application config template
├── roles/
│   └── webserver/               # Reusable role for nginx + custom site
│       ├── defaults/
│       │   └── main.yml
│       ├── handlers/
│       │   └── main.yml
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── index.html.j2
├── site.yml                     # Base configuration playbook (packages + dirs)
└── webserver.yml                # Web server configuration playbook
└── .github/
└── workflows/
└── ansible-ci.yml   # GitHub Actions CI (lint + dry-run)
text## How to Run Locally

1. **Install dependencies** (if not already done):

   ```bash
   pip install ansible ansible-lint
   brew install nginx     # optional, playbook installs it too

Dry-run (check mode):Bashansible-playbook site.yml --check --diff
ansible-playbook webserver.yml --check --diff
Apply changes (requires BECOME password for sudo):Bashansible-playbook site.yml --ask-become-pass
ansible-playbook webserver.yml --ask-become-pass
Verify the web server:
After successful run: open http://bootcamp-app.local:8080 (or http://localhost:8080)
Add to /etc/hosts if needed:textsudo sh -c 'echo "127.0.0.1 bootcamp-app.local" >> /etc/hosts'
Test nginx config:Bashnginx -t
brew services restart nginx
You should see a custom page: "Welcome to bootcamp-app" with environment info.

Key Decisions & Adaptations

Used hosts: localhost + ansible_connection=local for safe local testing on macOS
Nginx installed/configured via Homebrew (paths: /opt/homebrew/etc/nginx/servers/)
Port set to 8080 (Homebrew default, no sudo needed for low ports)
Custom server_name (bootcamp-app.local) to avoid conflict with default nginx welcome page
Reusable role: webserver for modularity
Jinja2 templates for dynamic content (index.html.j2)
Handlers use brew services restart nginx with become: false to avoid Homebrew root issues

CI/CD Integration
.github/workflows/ansible-ci.yml runs on push/PR:

Installs ansible & ansible-lint
Runs linting (ansible-lint)
Syntax-checks playbooks
Dry-runs (--check --diff) to catch issues early

Workflow should appear in the Actions tab after the first manual trigger or push.
Troubleshooting Tips

Conflicting server name warning? → Use unique server_name in conf
"Welcome to nginx!" page? → Ensure custom conf is loaded (restart nginx, check logs)
Handler fails? → Add become: false to handler in webserver.yml
Actions tab empty? → Manually "Run workflow" from the yml file page in GitHub


```

## ✅ Submission Requirements

1. **Ansible Playbooks**
   - Server configuration playbooks
   - Application deployment playbook
   - System update playbook

2. **Ansible Roles**
   - Reusable roles for common tasks
   - Proper role structure

3. **Inventory Management**
   - Multi-environment inventory
   - Dynamic inventory configuration

4. **CI/CD Integration**
   - Automated Ansible execution in GitHub Actions

5. **Documentation**
   - Playbook usage instructions
   - Role documentation

## Playbook Usage Instructions

### Prerequisites
- Ansible installed (`pip install ansible ansible-lint`)
- Homebrew installed on macOS (for nginx)
- BECOME (sudo) password ready for tasks requiring root privileges
- GitHub repo cloned and in the project directory

### Available Playbooks

1. **site.yml** – Base server configuration
   - Installs common packages: nginx, git, curl
   - Creates application directories under /opt and /var/www
   - Deploys a templated application config file
   - Idempotent: safe to run multiple times

   Dry-run:
   ```bash
   ansible-playbook site.yml --check --diff

Apply:
Bashansible-playbook site.yml --ask-become-pass

webserver.yml – Web server setup with custom site
Ensures nginx is installed
Creates document root (/var/www/bootcamp-app)
Deploys custom index.html from Jinja2 template
Configures nginx virtual host in /opt/homebrew/etc/nginx/servers/bootcamp-app.conf
Reloads nginx on changes (handler)
Dry-run:Bashansible-playbook webserver.yml --check --diffApply:Bashansible-playbook webserver.yml --ask-become-pass

### Verification Steps After Running webserver.yml

Check nginx configuration:Bashnginx -t
Restart nginx if needed:Bashbrew services restart nginx
Open in browser:
http://bootcamp-app.local:8080  (recommended)
or http://localhost:8080 (if server_name includes localhost)

Make sure /etc/hosts has:text127.0.0.1   bootcamp-app.local
Check logs:Bashtail -f /opt/homebrew/var/log/nginx/bootcamp-app_access.log
tail -f /opt/homebrew/var/log/nginx/bootcamp-app_error.log

###  result: Custom page showing "Welcome to bootcamp-app", environment "development", and "Managed by Ansible".
Role Documentation: webserver

### Purpose
Reusable Ansible role to configure a basic nginx web server with a custom document root and index page.

### Location
roles/webserver/
Files & Directories

defaults/main.yml – Default variable values
tasks/main.yml – Main tasks (install packages, create root, deploy template, ensure service)
handlers/main.yml – Handlers (restart nginx)
templates/index.html.j2 – Jinja2 template for custom welcome page

### Variables (Customizable)


Variable Name          |   Default Value|.                   Description                           |.    Required?             |
webserver_port         |.         80.   | Listening port (changed to 8080 in playbook).            |         No.               |
webserver_document_root| "/var/www/html |                   "Root directory for web files          | Yes (override recommended)|
webserver_server_name. |  "localhost    |                 "Server name in nginx config.            |         No                |
webserver_packages     |    ["nginx"]   |                Packages to install                       |         No                |
  

Variable NameDefault ValueDescriptionRequired?webserver_port80Listening port (changed to 8080 in playbook)Nowebserver_document_root"/var/www/html"Root directory for web filesYes (override recommended)webserver_server_name"localhost"Server name in nginx configNowebserver_packages["nginx"]Packages to installNo
In webserver.yml, we override some defaults:
YAMLvars:
  vhost_port: 8080
  document_root: "/var/www/{{ app_name }}"


### How the Role Works (Step by Step)

Installs nginx package
Creates the specified document root directory
Deploys index.html.j2 template to document root
Ensures nginx service is running (in full role; in playbook we use handler reload)

### Example Usage in Playbook
YAML- name: Apply webserver role
  hosts: webservers
  become: true
  roles:
    - role: webserver
      vars:
        webserver_document_root: "/var/www/{{ app_name }}"
        webserver_server_name: "bootcamp-app.local"

### Best Practices Used

Idempotent tasks (file, template, service modules)
Handler for service reload/restart
Jinja2 templating for dynamic content
Separated defaults for easy override

This role can be reused in other projects by copying the roles/webserver folder or publishing to Ansible Galaxy (future enhancement).
text





## 🎓 Grading Rubric

| Criteria | Points |
|----------|--------|
| **Playbook Development** | 30 |
| **Role Structure** | 25 |
| **CI/CD Integration** | 25 |
| **Documentation** | 20 |
| **Total** | 100 |

## 💡 Tips

- Test playbooks with `--check` flag first
- Use Ansible Vault for sensitive data
- Implement idempotency in all tasks
- Start with simple playbooks, then modularize into roles

## 📚 Resources

- [Ansible Documentation](https://docs.ansible.com/)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

## 🚀 Submission

**Reminder:** After pushing your work and opening a PR:
- Copy the **PR URL**
- Paste it into the **Lab Submission** field in the Student Portal
