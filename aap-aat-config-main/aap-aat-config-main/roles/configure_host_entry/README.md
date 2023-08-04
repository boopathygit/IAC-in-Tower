# configure_inventory

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) hosts

## Requirements

Version: 2.9+

### Modules
  - ansible.controller | https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html

### Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Variables

### Authentication

| Name                | Type   | Description                                   | Default           |
| ------------------- | ------ | --------------------------------------------- | ----------------- |
| controller_hostname | string | The host used for run role's tasks            | aap.ascension.org |
| controller_user     | string | Username for accessing controller_hostname    | null              |
| controller_pass     | string | Password for accessing controller_hostname    | null              |
| controller_token    | raw    | OAuth token for accessing controller_hostname | null              |

### Role-specific

| Name                      |   Type    | Description                                                    | Default |
| ------------------------- | :-------: | -------------------------------------------------------------- | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation         |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                 |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                 |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                 |    1    |
| host_entries              | list(map) | A list of hosts to configure within AAP. *See structure below* |   []    |


### Formating Variables
Variables can use a standard Jinja templating format to describe the resource.

Example:
```json
{{ variable }}
```

Because of this it is difficult to provide controller with the required format for these fields.

The workaround is to use the following format:
```json
{  { variable }}
```
The role will strip the double space between the curly bracket in order to provide controller with the correct format for the Variables.

## Data Structure

| Variable Name |  Type  | Description                             | Choice / Default                                         |
| ------------- | :----: | --------------------------------------- | -------------------------------------------------------- |
| `name`        | string | The name of the host.                   |                                                          |
| `new_name`    | string | To use when changing a hosts's name.    | ""                                                       |
| `description` | string | The description of the host.            | ""                                                       |
| `inventory`   | string | The inventory the host applies against. |                                                          |
| `enabled`     |  bool  | If the host should be enabled.          | **CHOICES**:<ul><li>**true**</li><li>false</li></ul>     |
| `variables`   | string | The variables applicable to the host.   | `{}`                                                     |
| `state`       | string | Desired state of the resource.          | **CHOICES**:<ul><li>**present**</li><li>absent</li></ul> |

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - host_entries.yml

  roles:
    - role: configure_host_entry
```

## Additional Notes

N/A