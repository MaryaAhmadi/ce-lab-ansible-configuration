# Lab M5.07 - Configuration Management with Ansible

**Cloud Engineering Bootcamp - Week 5, Day 4**  
**Module:** Cloud Automation & CI/CD

## Start Here: Fork, Clone, and Submit

You will complete this lab by working in **your own fork** of the lab repository and submitting a **Pull Request (PR)**.

1. **Fork the lab repository** to your GitHub account.
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/<your-github-username>/ce-lab-analyzing-costs-cost-explorer.git
   cd ce-lab-analyzing-costs-cost-explorer
   ```
3. **Follow all instructions below** and save your work in this repo (files, screenshots, and notes).
4. **When finished, submit your work:**
   - `git add` → `git commit` → `git push`
   - Open a **Pull Request** from your fork back to the original lab repo
   - Copy the **PR URL** and paste it into the **Lab Submission** field in the Student Portal

## 📋 Lab Overview

Use Ansible to automate server configuration management, application deployment, and system updates across multiple EC2 instances.

## 🎯 Learning Objectives

- Write Ansible playbooks for configuration management
- Manage inventory files for multiple environments
- Use Ansible roles for code reusability
- Integrate Ansible with GitHub Actions
- Implement idempotent configuration management

## 📁 Repository Structure

```
ce-lab-ansible-configuration/
├── .github/
│   └── workflows/
│       └── ansible-deploy.yml
├── ansible/
│   ├── playbooks/
│   │   ├── webserver.yml
│   │   └── database.yml
│   ├── roles/
│   │   ├── common/
│   │   ├── nginx/
│   │   └── application/
│   ├── inventory/
│   │   ├── dev.ini
│   │   └── prod.ini
│   └── ansible.cfg
├── README.md
└── .gitignore
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
