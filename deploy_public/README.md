# Ansible Playbook for SSH Key Distribution and User Management

This Ansible playbook is designed to distribute SSH public keys to specified servers and manage user accounts. It ensures that the necessary directories and files are in place for SSH key authentication and adds the public keys to the authorized_keys file for each user.

## Requirements

* Ansible 2.9 or higher
* SSH access to the target servers
* The `keys.yml` file containing the public keys and user information

## How to Use

1. Ensure the `keys.yml` file in the `deploy_public` directory contains the public keys and user information for each server. The format should be as follows:
```yaml
user_pub_keys:
  <server_ip>:
  - user: <username>
    key: "<ssh_public_key>"
```

2. Run the playbook using the following command:
```
ansible-playbook -i deploy_public/hosts deploy_public/deploy_public.yaml --ask-vault-pass --ask-become-pass
```

3. The playbook will distribute the SSH public keys to the specified servers.

## Playbook Details

The playbook consists of two main tasks:

1. **Ensure the .ssh directory exists**: This task ensures that the `.ssh` directory exists for each user specified in the `keys.yml` file. It sets the correct permissions and ownership for the directory.
2. **Add the public key to authorized_keys**: This task adds the public key for each user to the `authorized_keys` file. This allows the user to authenticate using their SSH key.

If the `users.yml` file is present, the playbook will also perform the following tasks:

1. **Manage user accounts**: This task ensures that the specified user accounts exist on the target servers. It sets the correct password and adds the user to the specified groups.

## Security Considerations

* Ensure that the `keys.yml` and `users.yml` files are kept secure and not committed to a public version control system.
* Use secure password hashes in the `users.yml` file.
* Limit access to the servers and the Ansible control machine to prevent unauthorized access.

## Troubleshooting

* Check the Ansible logs for errors during the playbook execution.
* Verify that the SSH keys are correctly formatted and that the user information in the `keys.yml` file matches the actual user accounts on the servers.
* Ensure that the Ansible control machine has SSH access to the target servers.





