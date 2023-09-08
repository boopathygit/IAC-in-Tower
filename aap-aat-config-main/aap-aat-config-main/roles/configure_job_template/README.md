**# configure_job_template

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) job templates

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

| Name                      |   Type    | Description                                                            | Default |
| ------------------------- | :-------: | ---------------------------------------------------------------------- | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation                 |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                         |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                         |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                         |    1    |
| job_templates             | list(map) | A list of job templates to configure within AAP. *See structure below* |   []    |

## Data Structure

| Name                           |     Type     | Description                                                                                                                                 | Choices / Default                                                           |
| ------------------------------ | :----------: | ------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| name                           |    string    | Name to use for the job template.                                                                                                           |                                                                             |
| new_name                       |    string    | Setting this option will change the existing name (looked up via the name field).                                                           | ""                                                                          |
| copy_from                      |    string    | Name or id to copy the job template from.<br><br>This will copy an existing job template and change any parameters supplied.                | ""                                                                          |
| description                    |    string    | Description to use for the job template.                                                                                                    | ""                                                                          |
| execution_environment          |    string    | Execution Environment to use for the job template.                                                                                          | ""                                                                          |
| job_type                       |    string    | The job type to use for the job template.                                                                                                   | **CHOICES**:<ul><li>**run**</li><li>check</li></ul>                         |
| inventory                      |    string    | Name of the inventory to use for the job template.                                                                                          | ""                                                                          |
| project                        |    string    | Name of the project to use for the job template.                                                                                            | ""                                                                          |
| playbook                       |    string    | Path to the playbook to use for the job template within the project provided.                                                               | ""                                                                          |
| credentials                    | list(string) | List of credentials to use for the job template.                                                                                            | []                                                                          |
| forks                          |    string    | The number of parallel or simultaneous processes to use while executing the playbook.                                                       | ""                                                                          |
| limit                          |    string    | A host pattern to further constrain the list of hosts managed or affected by the playbook                                                   | ""                                                                          |
| verbosity                      |   integer    | Control the output level Ansible produces as the playbook runs. 0 - Normal, 1 - Verbose, 2 - More Verbose, 3 - Debug, 4 - Connection Debug. | **CHOICES**:<ul><li>**0**</li><li>1</li><li>2</li><li>3</li><li>4</li></ul> |
| extra_vars                     |  dictionary  | Specify extra_vars for the template.                                                                                                        | ""                                                                          |
| job_tags                       |    string    | Comma separated list of the tags to use for the job template.                                                                               | ""                                                                          |
| force_handlers                 |   boolean    | Enable forcing playbook handlers to run even if a task fails.                                                                               | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| skip_tags                      |    string    | Comma separated list of the tags to skip for the job template.                                                                              | ""                                                                          |
| start_at_task                  |    string    | Start the playbook at the task matching this name.                                                                                          | ""                                                                          |
| diff_mode                      |   boolean    | Enable diff mode for the job template.                                                                                                      | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| use_fact_cache                 |   boolean    | Enable use of fact caching for the job template.                                                                                            | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| host_config_key                |    string    | Allow provisioning callbacks using this host config key.                                                                                    |                                                                             |
| ask_scm_branch_on_launch       |   boolean    | Prompt user for (scm branch) on launch.                                                                                                     | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_diff_mode_on_launch        |   boolean    | Prompt user to enable diff mode (show changes) to files when supported by modules                                                           | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_variables_on_launch        |   boolean    | Prompt user for (extra_vars) on launch.                                                                                                     | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_limit_on_launch            |   boolean    | Prompt user for a limit on launch.                                                                                                          | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_tags_on_launch             |   boolean    | Prompt user for job tags on launch.                                                                                                         | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_skip_tags_on_launch        |   boolean    | Prompt user for job tags to skip on launch.                                                                                                 | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_job_type_on_launch         |   boolean    | Prompt user for job type on launch.                                                                                                         | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_verbosity_on_launch        |   boolean    | Prompt user to choose a verbosity level on launch.                                                                                          | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_inventory_on_launch        |   boolean    | Prompt user for inventory on launch.                                                                                                        | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| ask_credential_on_launch       |   boolean    | Prompt user for credential on launch.                                                                                                       | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| survey_enabled                 |   boolean    | Enable a survey on the job template.                                                                                                        | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| survey_spec                    |  dictionary  | JSON/YAML dict formatted survey definition.                                                                                                 | ""                                                                          |
| become_enabled                 |   boolean    | Activate privilege escalation.                                                                                                              | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| allow_simultaneous             |   boolean    | Allow simultaneous runs of the job template.                                                                                                | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>                        |
| timeout                        |   integer    | Maximum time in seconds to wait for a job to finish (server-side).                                                                          | ""                                                                          |
| instance_groups                | list(string) | List of Instance Groups for this Organization to run on.                                                                                    | ""                                                                          |
| job_slice_count                |    string    | The number of jobs to slice into at runtime. Will cause the Job Template to launch a workflow if value is greater than 1.                   | ""                                                                          |
| webhook_service                |    string    | Service that webhook requests will be accepted from                                                                                         | **CHOICES**:<ul><li>github</li><li>gitlab</li></ul>                         |
| webhook_credential             |    string    | Personal Access Token for posting back the status to the service API                                                                        | ""                                                                          |
| scm_branch                     |    string    | Branch to use in job run. Project default used if blank. *Only allowed if project allow_override field is set to true.*                     | ""                                                                          |
| labels                         | list(string) | The labels applied to this job template<br>Must be created with the labels module first. This will error if the label has not been created. | ""                                                                          |
| state                          |    string    | Desired state of the job template.                                                                                                          | **CHOICES**:<ul><li>present</li><li>absent</li></ul>                        |
| notification_templates_started | list(string) | List of notifications to send on start                                                                                                      | ""                                                                          |
| notification_templates_success | list(string) | List of notifications to send on success                                                                                                    | ""                                                                          |
| notification_templates_error   | list(string) | List of notifications to send on failure                                                                                                    | ""                                                                          |

## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - templates.yml

  roles:
    - role: configure_job_template
```

## Additional Notes

N/A
