# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]
### Added
- Initial project scaffold bootstrapped from automation/bootstrap

## [Unreleased] fix/hardcoded-ports-and-ips
### Fixed
- Replace all hardcoded port numbers and IP addresses with variables (`rabbitmq_amqp_port`,
  `rabbitmq_amqp_ssl_port`, `rabbitmq_management_port`, `rabbitmq_management_ssl_port`,
  `rabbitmq_prometheus_ssl_port`) so deployments can customise ports without patching templates
- `management.tcp.ip` in `rabbitmq.conf.j2` now uses `rabbitmq_management_listen_address`
  (default `127.0.0.1`) instead of a hardcoded literal
- `rabbitmqadmin` download URL and backup cronjob now use management address/port variables
  instead of `127.0.0.1:15672`
- All HAProxy template ports replaced with variables (`rabbitmq_haproxy_amqp_port`,
  `rabbitmq_haproxy_amqp_ssl_port`, `rabbitmq_haproxy_mgmt_port`, `rabbitmq_haproxy_mgmt_ssl_port`,
  `rabbitmq_haproxy_stats_port`, `rabbitmq_haproxy_log_address`)
- `tasks/users.yml` and `tasks/vhosts.yml` node parameter now uses `rabbitmq_nodename` variable
  (default `rabbit@{{ ansible_hostname }}`) consistent with `rabbitmq-env.conf.j2`, replacing
  hardcoded `ansible_fqdn` which was wrong for non-longname deployments
- `tasks/queues.yml` `login_host` fallback changed from `ansible_fqdn` to `inventory_hostname`;
  `login_port` now uses port variables instead of inline literals
