# configure_inventory

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) inventories

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
| job_templates             | list(map) | A list of inventories to configure within AAP. *See structure below* |   []    |


## Data Structure

| Name            | Type         | Description                                                                                                                                                                                   | Choice / Default                                         |
| --------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| name            | string       | The name to use for the inventory.                                                                                                                                                            |                                                          |
| copy_from       | string       | Name or id to copy the inventory from.<br>This will copy an existing inventory and change any parameters supplied.<br> The new inventory name will be the one provided in the name parameter. | ""                                                       |
| description     | string       | The description to use for the inventory.                                                                                                                                                     | ""                                                       |
| organization    | string       |                                                                                                                                                                                               | "AscTech"                                                |
| instance_groups | list(string) | List of Instance Groups for this Organization to run on.                                                                                                                                      | ""                                                       |
| variables       | dictionary   |                                                                                                                                                                                               | []                                                       |
| kind            | string       | The type of inventory to create                                                                                                                                                               | **CHOICES**:<ul><li>***empty***</li><li>smart</li></ul>  |
| host_filter     | string       | The host_filter field. Only useful when kind=smart                                                                                                                                            |                                                          |
| state           | string       | Desired state of the job template.                                                                                                                                                            | **CHOICES**:<ul><li>**present**</li><li>absent</li></ul> |

## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - inventories.yml

  roles:
    - role: configure_inventory
```

## Additional Notes

N/A
