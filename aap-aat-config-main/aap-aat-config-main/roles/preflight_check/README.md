# preflight_check

Ansible role to validate AAP authentication and team

## Requirements

Version: 2.9+

### Modules
  - ansible.controller | https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html

## Variables

### Authentication

| Name                | Type   | Description                                                                   | Default           |
| ------------------- | ------ | ----------------------------------------------------------------------------- | ----------------- |
| controller_hostname | string | The host used for run role's tasks                                            | aap.ascension.org |
| controller_user     | string | Username for accessing controller_hostname                                    | null              |
| controller_pass     | string | Password for accessing controller_hostname                                    | null              |
| controller_token    | raw    | OAuth token for accessing controller_hostname                                 | null              |
| TOKEN_STRING        | raw    | Ansible token for accessing controller_hostname (Specific to current AAP env) | null              |

### Role-specific

| Name                      |  Type   | Description                                            | Default |
| ------------------------- | :-----: | ------------------------------------------------------ | :-----: |
| job_config_secure_logging | boolean | whether to enable secure logging during role operation |  false  |
| job_config_ignore_errors  | boolean | whether to ignore errors during role operation         |  false  |
| async_config_retries      | integer | How many retries to use during async operation         |   30    |
| async_config_delay        | integer | How many seconds to wait between async retries         |    1    |
| aap_team                  | string  | The Ansible Automation Platform team name to verify.   |   ""    |

## Dependencies

- Read/Write access to target AAP organization
- Username / password OR Token access to AAP
- HTTPS access to AAP

## Example Playbook

```yaml
---
# Authentication using username / password
- hosts: localhost
  gather_facts: false
  roles:
    - role: preflight_check
      vars:
        aap_team: some_name_here
        controller_user: some_user
        controller_pass: some_pass_value
```

```yaml
---
# Pass value from TOKEN_STRING to controller_token, otherwise use default value
- hosts: localhost
  gather_facts: false
  roles:
    - role: preflight_check
      vars:
        aap_team: some_name_here
        controller_token: "{{ TOKEN_STRING | default('some_token_value', true) }}"
```

## Additional Notes

N/A