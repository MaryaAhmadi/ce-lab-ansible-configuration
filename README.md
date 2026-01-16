# Lab M5.07 - Configuration Management with Ansible

**Cloud Engineering Bootcamp - Week 5, Day 4**  
**Module:** Cloud Automation & CI/CD

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

Submit your repository URL through the course platform.
