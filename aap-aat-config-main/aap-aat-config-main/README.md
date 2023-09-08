# AAP Configuration - AAT

Ansible playbook with roles for performing move / add / changes against Ansible Automation Platform (AAP). Repository is designated for AAT.

[![aat-config](https://img.shields.io/badge/automation_platform-aat_config_job-red?style=for-the-badge&logo=ansible)](https://boo.tower.com/#/templates/job_template/364/jobs)

## Requirements

- Version: 2.9+
- [AAP Onboarding Process](https://github.********.org/******/aap-base-config/blob/main/docs/README.md)
- [Github Naming Process](https://docs.google.com/document/d/1EfCEjD3Af9k67AY6i4rtcoLrQzeTWqW0B9uZwUhRNnY/edit?usp=sharing) Please read for naming convention of your repo.

> After performing the AAP onboarding process, [line 3 of CODEOWNERS](CODEOWNERS) should be updated with your team's GitHub accounts.

```yaml
# FROM
# *                   @:github_account @github_account2 etc

# TO
*                   @teammbr1 @teammbr2 @teammbr3
```

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

### Playbook-specific

| Name                      |   Type    | Description                                                                                            | Default |
| ------------------------- | :-------: | ------------------------------------------------------------------------------------------------------ | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation                                                 |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                                                         |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                                                         |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                                                         |    1    |
| aap_team                  |  string   | The team name in AAP. Used for configuring basic permissions                                           |         |
| job_templates             | list(map) | A list of job templates to configure within AAP. [*See structure below*](#job_templates)               |   []    |
| host_entries              | list(map) | A list of host_entries to configure within AAP. [*See structure below*](#host_entries)                 |   []    |
| workflow_templates        | list(map) | A list of workflow job templates to configure within AAP. [*See structure below*](#workflow_templates) |   []    |
| inventories               | list(map) | A list of inventories to configure within AAP. [*See structure below*](#inventories)                   |   []    |
| projects                  | list(map) | A list of projects to configure within AAP. [*See structure below*](#projects)                         |   []    |
| credentials               | list(map) | A list of credentials to configure within AAP. [*See structure below*](#credentials)                   |   []    |
| rbac                      | list(map) | A list of role-based access controls to configure within AAP. [*See structure below*](#rbac)           |   []    |


## Data Structure

### job_templates

> All configurable options may be found in the [role's readme](roles/configure_job_template/README.md#data-structure) file

```yaml
# Basic Job Template

job_templates:
    - name: test_job_template
      description: 'Test Job Template associated with SCM project'
      credentials:
        - Test_Credential_1
      playbook: test_playbook.yml
      inventory: test_inventory
      project: test_scm_project
```

### host_entries

> All configurable options may be found in the [role's readme](roles/configure_host_entry/README.md#data-structure) file

```yaml
# Basic Host Entry

host_entries:
    - name: test_host_1
      description: 'Test Job Template associated with SCM project'
      inventory: test_inventory_1
```

### workflow_templates

> All configurable options may be found in the [role's readme](roles/configure_workflow_template/README.md#data-structure) file

```yaml
workflow_templates:

# Create workflow using simplified_schema parameter
  - name: test_wkflw_with_approval-simple
    simplified_schema:
      - identifier: approve_job
        approval_node:
          name: approve_job
        success_node:
          - cfg_endpoint
        failure_node:
          - notify_team
      - identifier: cfg_endpoint
        unified_job_template: cfg_endpoint
      - identifier: notify_team
        unified_job_template: notify_team

# Create workflow using schema parameter
  - name: test_wkflw_with_approval
    schema:
      - identifier: approve_job
        related:
          success_nodes:
            - identifier: cfg_endpoint
          failure_nodes:
            - identifier: notify_team
      - identifier: cfg_endpoint
        unified_job_template:
          name: aap_node_configuration
          organization:
            name: 'AscTech'
          type: job_template
      - identifier: notify_team
        unified_job_template:
          name: notify_team
          organization:
            name: 'AscTech'
          type: job_template
```

### inventories

> All configurable options may be found in the [role's readme](roles/configure_inventory/README.md#data-structure) file

```yaml
# Basic Inventory

inventories:
    - name: test_inventory
      description: 'Test inventory used with test_job_template'
```

### projects

> All configurable options may be found in the [role's readme](roles/configure_project/README.md#data-structure) file

```yaml
# Basic SCM project

projects:
    - name: test_scm_project
      description: 'Ansible project connected to SCM'
      scm_url: 'https://github.ascension.org/Ascension/test-ansible-repo'

```

### credentials

> All configurable options may be found in the [role's readme](roles/configure_credential/README.md#data-structure) file

```yaml
# Basic Machine credential

credentials:
    - name: test_machine_credential
      description: 'Ansible credential for accessing machines'
```

### rbac

> All configurable options may be found in the [role's readme](roles/configure_rbac/README.md#data-structure) file

```yaml
# Grant Team execute access to job template

rbac:
    - role: execute
      team: test_team
      job_templates:
        - test_template

```


## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP
- Valid AAP team name

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - inventories.yml

  roles:
    - role: configure_inventory
```

## Contributing / Using the repository

Contributing guidelines may be [found here](CONTRIBUTING.md)

## Additional Notes

- The pre_task section in [main.yml](main.yml) is for validating the configuration prior to application. If there are any issues with using this repository,
please reach out to the [Solutions Architect team][sol-arch-email] for assistance.
- For best results, its recommended to use a [GitHub webhook][ghe-webhook] with AAP

<!-- References -->
[sol-arch-email]: mailto:SolutionsArchitectureIaaSTeam@ascension.org
[ghe-webhook]: https://docs.ansible.com/automation-controller/latest/html/userguide/webhooks.html#github-webhook-setup
