Role Name
=========

Installs AWS CloudWatch Log Agent

Requirements
------------

Requires ec2_facts.

Role Variables
--------------

`logs`, `extra_logs`: list of logs with the following keys:

| Name            | Description                | Required | Default
|-----------------|----------------------------|----------|---------
| file            | Full path to log file      | Yes      |
| datetime_format | Datetime format            | No       | None
| log_group_name  | CloudWatch Log Group       | Yes      |
| log_stream_name | CloudWatch Log Stream Name | No       | The instance id

`daemon_name`: Optional AWS log daemon service name, e.g. "awslogsd" for Amazon
Linux 2

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

    - hosts: servers
      vars:
        logs:
          - file: /var/log/audit/audit.log
            # auditd's timestamps are in epoch-seconds, which do appear to be parseable using awslogs datetime_format
            # datetime_format: "%b %d %H:%M:%S"
            log_group_name: "audit"
            log_stream_name: "audit-stream"
          - file: /home/ubuntu/.bash_history
            log_group_name: "bash_history"
        awslogs_loglevel: info
        daemon_name: "awslogsd"
      pre_tasks:
        - ec2_metadata_facts:
      roles:
         - { role: dharrisio.aws-cloudwatch-logs }

License
-------

GPLv3

Author Information
------------------

Created by David Harris
