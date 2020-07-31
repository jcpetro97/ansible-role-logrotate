# Logrotate

- [Logrotate](#logrotate)
  - [Tags](#tags)
  - [Settings](#settings)
  - [Examples](#examples)

Configures logrotate scripts for log files. Comes with some builtin configs
and allows for custom configs.


## Tags

This role uses two tags: **build** and **configure**

* `build` - Installs Logrotate server.
* `configure` - Configures and ensures that the service is running.


## Settings

|Variable|Default|Description|
|---|---|---|
|logrotate_application_logs_paths|[]|Out of the box config for generic application logs|
|logrotate_custom_configs|[]|Specify a path and a list of options to go into the config file|
|logrotate_version|~|Version number Logrotate package|


## Examples

**Note** You can force logrotate to run like so:

```BASH
# Dry run
logrotate -d /etc/logrotate.conf
# Actual run
logrotate -d -f /etc/logrotate.conf
```

Install with builtin application log config and custom config:

```YAML
- name: Install Logrotate
  hosts: sandbox

  roles:
    role: logrotate
    logrotate_application_logs_paths:
      - /application/logs/*.log
      - /another_application/logs/*.log
    logrotate_custom_configs:
      - name: custom_log
        path: "/var/log/cutom/*.log"
        options:
          - weekly
          - size 25M
          - missingok
          - compress
          - delaycompress
          - copytruncate
        scripts:
          postrotate: "service rsyslog restart"
```
