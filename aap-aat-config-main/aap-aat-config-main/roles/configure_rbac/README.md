# configure_rbac

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) Role-Based Access Controls (RBAC)

## Requirements

Version: 2.9+

### Modules
  - ansible.controller | https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html

## Variables

### Authentication

| Name                | Type   | Description                                   | Default           |
| ------------------- | ------ | --------------------------------------------- | ----------------- |
| controller_hostname | string | The host used for run role's tasks            | aap.ascension.org |
| controller_user     | string | Username for accessing controller_hostname    | null              |
| controller_pass     | string | Password for accessing controller_hostname    | null              |
| controller_token    | raw    | OAuth token for accessing controller_hostname | null              |

### Role-specific

| Name                      |   Type    | Description                                                                         | Default |
| ------------------------- | :-------: | ----------------------------------------------------------------------------------- | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation                              |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                                      |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                                      |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                                      |    1    |
| rbac                      | list(map) | A list of role-based access controls to configure within AAP. *See structure below* |   []    |

## Data Structure

| Name                |     Type     | Description                                                                                                                                                                                                                                                 | Choice / Default                                                                                                        |
| ------------------- | :----------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| user                |    string    | User that receives the permissions specified by the role.                                                                                                                                                                                                   |                                                                                                                         |
| team                |    string    | Team that receives the permissions specified by the role.                                                                                                                                                                                                   |                                                                                                                         |
| role                |    string    | The role type to grant/revoke.                                                                                                                                                                                                                              | **Choices**<ul><li>admin</li><li>read</li><li>member</li><li>execute</li><li>adhoc</li><li>update</li><li>use</li></ul> |
| inventories         | list(string) | Inventorie(s) the role acts on.                                                                                                                                                                                                                             |                                                                                                                         |
| job_templates       | list(string) | Job template(s) the role acts on.                                                                                                                                                                                                                           |                                                                                                                         |
| workflows           | list(string) | Workflow(s) the role acts on.                                                                                                                                                                                                                               |                                                                                                                         |
| credentials         | list(string) | Credential(s) the role acts on.                                                                                                                                                                                                                             |                                                                                                                         |
| projects            | list(string) | Project(s) the role acts on.                                                                                                                                                                                                                                |                                                                                                                         |
| lookup_organization |    string    | Organization the inventories, job templates, projects, or workflows the items exists in.<br>Used to help lookup the object, for organization roles see organization.<br><br>If not provided, will lookup by name only, which does not work with duplicates. |                                                                                                                         |
| state               |    string    | Desired state of the resource.                                                                                                                                                                                                                              | **CHOICES**:<ul><li>**present**</li><li>absent</li></ul>                                                                |

### Role options

| Resource Type | Options                                                                       |
| ------------- | ----------------------------------------------------------------------------- |
| inventories   | <ul><li>admin</li><li>update</li><li>use</li><li>adhoc</li><li>read</li></ul> |
| job templates | <ul><li>admin</li><li>use</li><li>read</li></ul>                              |
| workflows     | <ul><li>admin</li><li>use</li><li>read</li></ul>                              |
| credentials   | <ul><li>admin</li><li>use</li><li>read</li></ul>                              |
| projects      | <ul><li>admin</li><li>update</li><li>use</li><li>read</li></ul>               |

## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - rbac.yml

  roles:
    - role: configure_rbac
```

## Additional Notes

N/A