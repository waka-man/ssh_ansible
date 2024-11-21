# Deploy Users

This repository contains an Ansible playbook for creating users on multiple hosts, adding their SSH keys, and configuring their environments. This playbook is designed to automate the process of user management, ensuring consistency and security across all managed hosts.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Playbook Details](#playbook-details)
- [Variables](#variables)
- [Handlers](#handlers)
- [License](#license)

## Prerequisites

Before using this playbook, ensure you have the following:
- Ansible installed on your control machine.
- SSH access to the target hosts with appropriate privileges.
- A `users.yml` file containing the user details.

## Usage

1. Clone this repository to your control machine:
   ```sh
   git clone https://github.com/Irembo/irembogov_3_sysops/ansible_palybooks/deploy_users.git
   cd deploy_users
   ```

2. Create a `users.yml` file in the same directory with the following structure:
   ```yaml
   users:
     - username: "user1"
       password: "password1"
       public_key: "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAr..."
       groups: "group1,group2"
     - username: "user2"
       password: "password2"
       public_key: "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAr..."
       groups: "group1,group3"
   ```

3. Run the playbook:
   ```sh
   ansible-playbook -i hosts deploy_users.yml --ask-vault-pass --ask-become-pass
   ```

## Playbook Details

The playbook `deploy_users.yml` performs the following tasks:

1. **Check if users exist, create if they don't**:
   - Uses the `user` module to create users with specified usernames and passwords.

2. **Ensure users' .ssh directory exists**:
   - Uses the `file` module to create the `.ssh` directory in each user's home directory with appropriate permissions.

3. **Add provided public keys to authorized_keys**:
   - Uses the `ansible.posix.authorized_key` module to add the provided public keys to the `authorized_keys` file for each user. This module was previously just the `authorized_key` module and now moved under the collection `ansible.posix`. This should be installed on the control machine with `ansible-galaxy collection install ansible.posix`

4. **Ensure users' home directory has correct permissions**:
   - Uses the `file` module to set the correct permissions for each user's home directory.

5. **Add users to appropriate groups**:
   - Uses the `user` module to add users to specified groups.

6. **Disable password authentication for users**:
   - Uses the `lineinfile` module to disable password authentication in the SSH configuration file.

## Variables

The playbook uses the following variables defined in the `users.yml` file:

- `username`: The username of the user to be created.
- `password`: The password for the user.
- `public_key`: The public SSH key to be added to the user's `authorized_keys` file.
- `groups`: The groups to which the user should be added.

## Handlers

The playbook includes a handler to restart the SSH service if the SSH configuration file is modified:

- **Restart SSH**:
  - Uses the `service` module to restart the `sshd` service.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
