# configure_project

Ansible role for performing move / add / changes against Ansible Automation Platform (AAP) projects

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

| Name                      |   Type    | Description                                                       | Default |
| ------------------------- | :-------: | ----------------------------------------------------------------- | :-----: |
| job_config_secure_logging |  boolean  | whether to enable secure logging during role operation            |  false  |
| job_config_ignore_errors  |  boolean  | whether to ignore errors during role operation                    |  false  |
| async_config_retries      |  integer  | How many retries to use during async operation                    |   30    |
| async_config_delay        |  integer  | How many seconds to wait between async retries                    |    1    |
| projects                  | list(map) | A list of projects to configure within AAP. *See structure below* |   []    |

## Data Structure

| Name                           |     Type     | Description                                                                                                                                                                                                                                                                                                                                                 | Choice / Default                                         |
| ------------------------------ | :----------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| name                           |    string    | Name to use for the project.                                                                                                                                                                                                                                                                                                                                |                                                          |
| copy_from                      |    string    | Name or id to copy the project from.<br>This will copy an existing project and change any parameters supplied.<br>The new project name will be the one provided in the name parameter.                                                                                                                                                                      |                                                          |
| description                    |    string    | Description to use for the project.                                                                                                                                                                                                                                                                                                                         |                                                          |
| scm_type                       |    string    | Type of SCM resource.                                                                                                                                                                                                                                                                                                                                       |                                                          |
| scm_url                        |    string    | URL of SCM resource.                                                                                                                                                                                                                                                                                                                                        |                                                          |
| default_environment            |    string    | Default Execution Environment to use for jobs relating to the project.                                                                                                                                                                                                                                                                                      |                                                          |
| scm_branch                     |    string    | The branch to use for the SCM resource.                                                                                                                                                                                                                                                                                                                     |                                                          |
| scm_refspec                    |    string    | The refspec to use for the SCM resource.<br><br>Examples include:<ul><li>refs/*:refs/remotes/origin/*</li><li>refs/pull/62/head:refs/remotes/origin/pull/62/head</li></ul><br>**[For more information, refer to the Documentation.](https://aap.ascension.org/#/projects/221/edit:~:text=For%20more%20information%2C%20refer%20to%20the%20Documentation.)** |                                                          |
| credential                     |    string    | Name of the credential to use with this SCM resource.                                                                                                                                                                                                                                                                                                       |                                                          |
| scm_clean                      |   boolean    | Remove local modifications before updating.                                                                                                                                                                                                                                                                                                                 | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| scm_delete_on_update           |   boolean    | Remove the repository completely before updating.                                                                                                                                                                                                                                                                                                           | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| scm_track_submodules           |   boolean    | Track submodules latest commit on specified branch.                                                                                                                                                                                                                                                                                                         | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| scm_update_on_launch           |   boolean    | Before an update to the local repository before launching a job with this project.                                                                                                                                                                                                                                                                          | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| scm_update_cache_timeout       |   integer    | Cache Timeout to cache prior project syncs for a certain number of seconds.<br><br>*Only valid if scm_update_on_launch is to True*, otherwise ignored.                                                                                                                                                                                                      | ""                                                       |
| allow_override                 |   boolean    | Allow changing the SCM branch or revision in a job template that uses this project.                                                                                                                                                                                                                                                                         | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| timeout                        |   integer    | The amount of time (in seconds) to run before the SCM Update is canceled. A value of 0 means no timeout.<br><br>If waiting for the project to update this will abort after this amount of seconds                                                                                                                                                           |                                                          |
| organization                   |    string    | Name of organization for project.                                                                                                                                                                                                                                                                                                                           | "AscTech"                                                |
| state                          |    string    | Desired state of the resource.                                                                                                                                                                                                                                                                                                                              | **CHOICES**:<ul><li>**present**</li><li>absent</li></ul> |
| wait                           |   boolean    | Provides option (True by default) to wait for completed project sync before returning<br><br>Can assure playbook files are populated so that job templates that rely on the project may be successfully created                                                                                                                                             | **CHOICES**:<ul><li>**true**</li><li>false</li></ul>     |
| update_project                 |   boolean    | Force project to update after changes.<br><br>Used in conjunction with wait, interval, and timeout.                                                                                                                                                                                                                                                         | **CHOICES**:<ul><li>true</li><li>**false**</li></ul>     |
| interval                       |    float     | The interval to request an update from the controller.<br><br>Requires wait.                                                                                                                                                                                                                                                                                | 1                                                        |
| notification_templates_started | list(string) | List of notifications to send on start                                                                                                                                                                                                                                                                                                                      | ""                                                       |
| notification_templates_success | list(string) | List of notifications to send on success                                                                                                                                                                                                                                                                                                                    | ""                                                       |
| notification_templates_error   | list(string) | List of notifications to send on failure                                                                                                                                                                                                                                                                                                                    | ""                                                       |


## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
- hosts: localhost

  vars_files:
    - projects.yml

  roles:
    - role: configure_project
```

## Additional Notes

N/A