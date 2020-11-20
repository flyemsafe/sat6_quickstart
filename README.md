# Satellite QuickStart Ansible Project

This ansible project use the [Red Hat Satellite Collections](https://cloud.redhat.com/ansible/automation-hub/repo/published/redhat/satellite) to install and configure Red Hat Satellite.

|  Playbooks | Purpose |
| --- | --- |
|  playbooks/setup_rhel_for_satellite.yml | Ensure your RHEL release is supported for running Satellite |
|  playbooks/install_satellite.yml | Install the Satellite server |
|  playbooks/configure_satellite.yml | Configure Satellite products, repositories, content views and activiation keys |

## How to use?
Review these two blog posts to understand how to setup your RHEL system or Ansible Tower to access cloud.redhat.com.
- [Hands on with Ansible collections](https://www.ansible.com/blog/hands-on-with-ansible-collections)
- [Installing and using collections on Ansible Tower](https://www.ansible.com/blog/installing-and-using-collections-on-ansible-tower)

If you execute the playbooks as job templates via Ansible Tower and have followed the Tower setup from the above blog post. The collections and roles will be downloaded automatically, assuming your Tower as access to the internet. If running from a RHEL ansible control node, follow the steps below to download the roles and collections.

**Collections**
1. Copy the `ansible.cfg.sample` to `ansible.cfg.sample` then update the galaxy section to include your auth token as per the blog post above. Make any other changes you would like.

2. Install the required collections
```
ansible-galaxy collection install -r collections/requirements.yml
```

**Roles**
Install the required roles
```
galaxy install -r roles/requirements.yml
```

**Vars**
Please review playbooks/vars/main.yml it is documented to help you understand what needs to be changed.

**Execute the playbooks**

Setup your RHEL host
```
ansible-playbook playbooks/setup_rhel_for_satellite.yml
```

Install Satellite
```
ansible-playbook playbooks/install_satellite.yml
```

Configure Satellite
```
ansible-playbook playbooks/configure_satellite.yml
```

