Initial Environment SetUp for Linux Servers
=========

This Ansible role is designed to perform the initial setup for Linux servers that will be used in development, production, and staging environments. It ensures that the servers are configured with the necessary packages, users, and settings to meet the requirements of these environments. The role handles tasks such as updating the system, installing essential packages, configuring security settings, and setting up user accounts and permissions. By using this role, you can ensure a consistent and secure setup across all your Linux servers, regardless of the environment they are deployed in.

Requirements
------------


Role Variables
--------------
The following variables can be set for this role:

- `update_system`: (Default: `true`) Whether to update the system packages.
- `install_packages`: (Default: `[]`) A list of packages to be installed.
- `create_users`: (Default: `[]`) A list of user accounts to be created. Each user can have the following attributes:
  - `name`: The username.
  - `uid`: The user ID.
  - `groups`: A list of groups the user should belong to.
  - `shell`: The user's login shell.
- `configure_firewall`: (Default: `true`) Whether to configure the firewall settings.
- `firewall_rules`: (Default: `[]`) A list of firewall rules to be applied.
- `ssh_keys`: (Default: `[]`) A list of SSH keys to be added to the users' authorized keys.

Variables from `defaults/main.yml`:
- `default_package_list`: A list of default packages to be installed if `install_packages` is not specified.

Variables from `vars/main.yml`:
- `default_user_shell`: The default shell for new users.

Variables from other roles and/or the global scope:
- `hostvars`: Variables that can be accessed from other hosts in the inventory.
- `group_vars`: Variables that can be accessed from groups in the inventory.
- `role_defaults`: Default variables defined in other roles that can be overridden.

These variables allow you to customize the setup process to meet the specific needs of your environment.


Dependencies
------------
This role has the following dependencies on other roles hosted on Ansible Galaxy:

- `geerlingguy.firewall`: This role is used to configure the firewall settings. Ensure that the `configure_firewall` variable is set to `true` to enable firewall configuration. You can also specify custom firewall rules using the `firewall_rules` variable.
- `geerlingguy.security`: This role is used to apply security hardening to the server. It can be used in conjunction with this role to ensure a secure setup.
- `geerlingguy.users`: This role is used to manage user accounts. If you are using this role, ensure that the `create_users` variable is set with the appropriate user details.

Make sure to include these roles in your `requirements.yml` file:

```yaml
roles:
  - src: geerlingguy.firewall
  - src: geerlingguy.security
  - src: geerlingguy.users
```

These dependencies ensure that your servers are configured with the necessary security and user management settings.


Example Playbook
----------------
```yaml
- hosts: servers
  become: yes
  vars:
    update_system: true
    install_packages:
      - vim
      - git
      - htop
    create_users:
      - name: deploy
        uid: 1001
        groups: sudo
        shell: /bin/bash
    configure_firewall: true
    firewall_rules:
      - port: 22
        proto: tcp
        rule: allow
    ssh_keys:
      - user: deploy
        key: "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAr..."
  roles:
    - { role: your_username.initial_setup }
```

License
-------
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

Author Information
------------------
If you have any questions or need further information, you can reach out to the role authors:

- Name: Milad Yarmohammadi
- Email: milad.yarmohammadi94@example.com
- GitHub: [miladyarmohammadi](https://github.com/miladyarmohammadi)

Feel free to contact us for any issues or contributions.
