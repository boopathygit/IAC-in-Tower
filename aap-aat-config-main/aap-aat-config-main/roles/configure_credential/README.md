# configure_credential

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) credentials

## Requirements

Version: 2.9+

### Modules
  - ansible.controller | https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html

## Variables

### Authentication

| Name                | Type   | Description                                   | Default           |
| ------------------- | ------ | --------------------------------------------- | ----------------- |
| controller_hostname | string | The host used for run role's tasks            | boo.tower.com     |
| controller_user     | string | Username for accessing controller_hostname    | null              |
| controller_pass     | string | Password for accessing controller_hostname    | null              |
| controller_token    | raw    | OAuth token for accessing controller_hostname | null              |

### Role-specific

| Name                      |   Type    | Description                                                          | Default |
| ------------------------- | :-------: | -------------------------------------------------------------------- | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation               |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                       |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                       |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                       |    1    |
| credentials               | list(map) | A list of credentials to configure within AAP. *See structure below* |   []    |


## Data Structure

| Name            | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Choice / Default                                         |
| --------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| name            | string | The name to use for the inventory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                                          |
| new_name        | string | Setting this option will change the existing name (looked up via the name field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | ""                                                       |
| copy_from       | string | Name or id to copy the credential from.<br>This will copy an existing credential and change any parameters supplied.<br> The new credential name will be the one provided in the name parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | ""                                                       |
| description     | string | The description to use for the credential.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | ""                                                       |
| organization    | string | List of Instance Groups for this Organization to run on.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | "AscTech"                                                |
| credential_type | string | The credential type being created.<br><br>Can be a built-in credential type such as "Machine", or a custom credential type such as "My Credential Type"<br><br> Choices include Amazon Web Services, Ansible Galaxy/Automation Hub API Token, Centrify Vault Credential Provider Lookup, Container Registry, CyberArk AIM Central Credential Provider Lookup, CyberArk Conjur Secret Lookup, Google Compute Engine, GitHub Personal Access Token, GitLab Personal Access Token, HashiCorp Vault Secret Lookup, HashiCorp Vault Signed SSH, Insights, Machine, Microsoft Azure Key Vault, Microsoft Azure Resource Manager, Network, OpenShift or Kubernetes API Bearer Token, OpenStack, Red Hat Ansible Automation Platform, Red Hat Satellite 6, Red Hat Virtualization, Source Control, Thycotic DevOps Secrets Vault, Thycotic Secret Server, Vault, VMware vCenter, or a custom credential type | "Machine"                                                |
| user            | string | User that should own this credential.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | ""                                                       |
| team            | string | Team that should own this credential.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | ""                                                       |
| state           | string | Desired state of the job template.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | **CHOICES**:<ul><li>**present**</li><li>absent</li></ul> |

## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - credentials.yml

  roles:
    - role: configure_credential
```

## Additional Notes

N/A
