# Satellite QuickStart Ansible Project
This ansible project use the [Red Hat Satellite Collections](https://cloud.redhat.com/ansible/automation-hub/repo/published/redhat/satellite) to install and configure Red Hat Satellite.

## Overview
This projects aims to get you up and running with the Red Hat Satellite Server quickly. It does not attempt to accomplish everything you would want to do with your Satellite. For example, you should edit the activation keys created by this project to include additional details such as:
- Service Level
- Subscriptions

You should build upon this automation to fine tune it to your desire by investing the collections and roles used by this project.

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

### Collections
1. Copy the `ansible.cfg.sample` to `ansible.cfg` then update the galaxy section to include your auth token as per the blog post above. Make any other changes you would like.

2. Install the required collections
```
ansible-galaxy collection install -r collections/requirements.yml
```

### Roles
Install the required roles
```
galaxy install -r roles/requirements.yml
```

## Inventory
1. Update `inventory/host.yml` with your satellite connection details
2. Update the vars in `inventory/group_vars/sat/main.yml`

The vars file `inventory/group_vars/sat/main.yml` it is documented to help you understand what needs to be changed. You can declare all your sentive vars in a ansible vault as described below.

In this example I am using [Ansible vault](https://www.tecmint.com/use-ansible-vault-in-playbooks-to-protect-sensitive-data/), which I recommend. However, you can just leave the password in plaintext and it will work. Not secure though.

**Example creating a vault file**

Run:

```
ansible-vault create inventory/group_vars/all/vault.yml
```
This will first prompt you to enter a vault password.
Then open the file in vim, enter your sensitive data e.g.

```
ansible_password: my-secret-password
```

You can store your vault password in a file and expose it as a environment variable for your conveince
as documented here [Setting a default password source](https://docs.ansible.com/ansible/latest/user_guide/vault.html#setting-a-default-password-source).

If you are using Ansible Tower to run the playbooks. You can store your vault password as a credential in [Ansible Tower](https://docs.ansible.com/ansible-tower/latest/html/userguide/credentials.html#vault).

- [Concise Article About Using Ansible Vault](https://dzone.com/articles/managing-secrets-using-ansible-vault-and-tower)

### Execute the playbooks

If you are not using a vault password, use `ansible-playbook --ask-vault-pass` instead of `ansible-playbook`.

**Setup your RHEL host**
```
ansible-playbook playbooks/setup_rhel_for_satellite.yml
```

**Install Satellite**
```
ansible-playbook playbooks/install_satellite.yml
```

**Configure Satellite**
```
ansible-playbook playbooks/configure_satellite.yml
```

