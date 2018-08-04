Role Name
=========

Installs AWS CloudWatch Log Agent

Requirements
------------

Requires ec2_metadata_facts.

Role Variables
--------------

`logs`, `extra_logs`: list of logs with the following keys:

| Name                        | Description                       | Required  | Choices   | Default |
|-----------------------------|-----------------------------------|-----------|-----------|---------|
| `file`                      | Full path to log file             | Yes       |           |
| `datetime_format`           | Datetime format                   | No        |           | None
| `log_group_name`            | CloudWatch Log Group              | Yes       |           |
| `log_stream_name`           | CloudWatch Log Stream Name        | No        |           | The instance id
| `multi_line_start_pattern`  | Pattern identifying line start    | No        |           |
| `initial_end_of_file`       | Start reading at end of file      | No        |           |
| `encoding`                  | File encoding                     | No        | [see reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html) | `utf_8`
| `time_zone`                 | Timestamp time zone               | No        | LOCAL, UTC | `LOCAL`
| `daemon_name`               | log daemon service name           | No        | `awslogsd` for Amazon Linux 2 | `awslogs`

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
      pre_tasks:
        - ec2_metadata_facts:
      roles:
         - role: dharrisio.aws-cloudwatch-logs
           awslogs_loglevel: info
	   daemon_name: "awslogsd"
	   logs:
           - file: /var/log/audit/audit.log
             # auditd's timestamps are in epoch-seconds, which do NOT appear to
             # be parseable using any available datetime_format; so, we disable
             # the datetime_format, and defaults event time to the read() time.
             #
             # datetime_format: "%b %d %H:%M:%S"
             log_group_name: "audit"
             log_stream_name: "audit-stream"

           - file: /home/ubuntu/.bash_history
             log_group_name: "bash_history"

           - file: /var/log/example.log
             datetime_format: "%Y-%m-%d %H:%M:%s,%f"
             log_group_name: "mlp"
             log_stream_name: "mlp-1"
             multi_line_start_pattern: "^WARNING\\s+"
             initial_end_of_file: yes
             encoding: "utf_8"
             time_zone: "UTC"
     

License
-------

GPLv3

Author Information
------------------

Created by David Harris
