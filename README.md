Role Name
=========

Installs AWS CloudWatch Log Agent

Requirements
------------

Requires ec2_facts.

Role Variables
--------------

`logs`, `extra_logs`: list of logs with the following keys:

| Name                        | Description                       | Required  | Choices   | Default |
|-----------------------------|-----------------------------------|-----------|-----------|---------|
| `file`                      | Full path to log file             | Yes       |           |
| `format`                    | Datetime format                   | No        |           | None
| `group_name`                | CloudWatch Log Group              | Yes       |           |
| `stream_name`               | CloudWatch Log Stream Name        | No        |           | The instance id
| `multi_line_start_pattern`  | Pattern identifying line start    | No        |           |
| `initial_end_of_file`       | Start reading at end of file      | No        |           |
| `encoding`                  | File encoding                     | No        | [see reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html) | `utf_8`
| `time_zone`                 | Timestamp time zone               | No        | LOCAL, UTC | `LOCAL`

`awslogs_loglevel`: maximal log level for the Log Agent's logs itself
("debug", "info", "warning", "error" or "critical"). If this parameter is
not specified, no specific logging configuration will take place and the
default level (info) will be used. This parameter is very basic and does not
allow flexible logging configuration, its only goal is to change the amount
of logs going into the log agent's own logfile.

Dependencies
------------

This role has no dependencies.

Example Playbook
----------------

```yaml
- hosts: servers
  vars:
    logs:
      - file: /var/log/auth.log
        format: "%b %d %H:%M:%S"
        group_name: "auth"
        stream_name: "auth-stream"
      - file: /home/ubuntu/.bash_history
        group_name: "bash_history"
      - file: /var/log/example.log
        format: "%Y-%m-%d %H:%M:%s,%f"
        group_name: "mlp"
        stream_name: "mlp-1"
        multi_line_start_pattern: "^WARNING\\s+"
        initial_end_of_file: yes
        encoding: "utf_8"
        time_zone: "UTC"

    awslogs_loglevel: info
  roles:
     - { role: dharrisio.aws-cloudwatch-logs }
```

License
-------

GPLv3

Author Information
------------------

Created by David Harris
