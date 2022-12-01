[![CI](https://github.com/ngine-io/ansible-role-vernemq/actions/workflows/ci.yml/badge.svg)](https://github.com/ngine-io/ansible-role-vernemq/actions/workflows/ci.yml)

# Ansible Role: VerneMQ

Installs [VerneMQ MQTT broker](https://vernemq.com) on RedHat/RockyLinux/CentOS or Debian/Ubuntu Linux system.

> ***NOTE***: The role uses pre-built packages under the [VerneMQ end user license agreement](https://vernemq.com/end-user-license-agreement/) which you must accept before using the packages.

## Requirements

None.

## Installation

Via `requirements.yml`:

```yaml
---
# file: requirements.yml
roles:
  - name: ngine_io.vernemq
    version: v0.6.0
```

To install:

```
ansible-galaxy install -r requirements.yml
```

## Role Variables

```yaml
vernemq__version: 1.12.6.2
```
Version to install.

```yaml
vernemq__wait_for_port: 8888
```
Port to check, on which vernemq http is listen.

```yaml
vernemq__config_backup_enabled: false
```
Backup the config file before each change.

```yaml
vernemq__default_configs:
    accept_eula: "no"
    allow_anonymous: "off"
    allow_register_during_netsplit: "off"
    allow_publish_during_netsplit: "off"
    allow_subscribe_during_netsplit: "off"
    allow_unsubscribe_during_netsplit: "off"
    allow_multiple_sessions: "off"
    coordinate_registrations: "on"
    max_inflight_messages: 20
    max_online_messages: 1000
    max_offline_messages: 1000
    max_message_size: 0
    upgrade_outgoing_qos: "off"
    listener.max_connections: 10_000
    listener.nr_of_acceptors: 10
    listener.tcp.default: "0.0.0.0:1883"
    listener.vmq.clustering: "0.0.0.0:44053"
    listener.http.default: "0.0.0.0:{{ vernemq__wait_for_port }}"
    systree_enabled: "on"
    systree_interval: 20_000
    graphite_enabled: "off"
    graphite_host: "localhost"
    graphite_port: 2003
    graphite_interval: 20_000
    shared_subscription_policy: "prefer_local"
    plugins.vmq_passwd: "on"
    plugins.vmq_acl: "on"
    plugins.vmq_diversity: "off"
    plugins.vmq_webhooks: "off"
    plugins.vmq_bridge: "off"
    topic_max_depth: 10
    metadata_plugin: "vmq_swc"
    vmq_acl.acl_file: "/etc/vernemq/vmq.acl"
    vmq_acl.acl_reload_interval: 10
    vmq_passwd.password_file: "/etc/vernemq/vmq.passwd"
    vmq_passwd.password_reload_interval: 10
    vmq_diversity.script_dir: "/usr/share/vernemq/lua"
    vmq_diversity.auth_postgres.enabled: "off"
    vmq_diversity.postgres.ssl: "off"
    vmq_diversity.postgres.password_hash_method: "crypt"
    vmq_diversity.auth_cockroachdb.enabled: "off"
    vmq_diversity.cockroachdb.ssl: "on"
    vmq_diversity.cockroachdb.password_hash_method: "bcrypt"
    vmq_diversity.auth_mysql.enabled: "off"
    vmq_diversity.mysql.password_hash_method: "password"
    vmq_diversity.auth_mongodb.enabled: "off"
    vmq_diversity.mongodb.ssl: "off"
    vmq_diversity.auth_redis.enabled: "off"
    vmq_bcrypt.pool_size: 1
    log.console: "file"
    log.console.level: "info"
    log.console.file: "/var/log/vernemq/console.log"
    log.error.file: "/var/log/vernemq/error.log"
    log.syslog: "off"
    log.crash: "on"
    log.crash.file: "/var/log/vernemq/crash.log"
    log.crash.maximum_message_size: "64KB"
    log.crash.size: "10MB"
    log.crash.rotation: "$D0"
    log.crash.rotation.keep: 5
    nodename: "VerneMQ@127.0.0.1"
    distributed_cookie: "vmq"
    erlang.async_threads: 64
    erlang.max_ports: 262_144
    leveldb.maximum_memory.percent: 70
```
Default configs used and may be overwritten by the `vernemq__configs` dict.

```yaml
vernemq__configs: {}
```
Configs which overwrite default configs, see example playbook.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: vernemq
  vars:
    vernemq__configs:
      accept_eula: "yes"
      nodename: "VerneMQ@{{ ansible_default_ipv4.address }}"
      max_online_messages: 100_000
  roles:
    - role: ngine_io.vernemq
```

## License

MIT / Apache2

## Author Information

This role was created in 2022 by [René Moser](https://renemoser.net).
