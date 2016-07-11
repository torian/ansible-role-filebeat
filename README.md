# Ansible role for FileBeat

[![Build Status](https://travis-ci.org/torian/ansible-role-filebeat.svg)](https://travis-ci.org/torian/ansible-role-filebeat)

An Ansible Role that installs FileBeat on Red Hat/CentOS or Debian/Ubuntu.

## Tested On

  * EL / Centos (6 / 7)
  * Debian (Wheezy / Jessie)
  * Ubuntu (Trusty)

## Role Variables

Available variables are listed below, along with their default values as
definied in `defaults/main.yml`.

FileBeat user and group. If you run FileBeat with a user other than root make
sure your logs are readable by the FileBeat user. Add the FileBeat user to a
privileged group, with access to your logs.

On Ubuntu you would add the user to the `adm` group. On CentOS you can adjust
the permissions with the `setfacl` command, e.g. `sudo setfacl -m g:filebeat:r
<path>`.

    filebeat_user: root
    filebeat_group: root

Create the `filebeat` user and group.

    filebeat_create_user: true

FileBeat version to use.

    filebeat_version: 1.1.1

Start FileBeat at boot time.

    filebeat_start_at_boot: true

FileBeat version upgrade. This option allows package upgrades.

    filebeat_upgrade: false

FileBeat configuration file.

    filebeat_config_file: /etc/filebeat/filebeat.yml

FileBeat registry file.

    filebeat_config_registry_file: /var/lib/filebeat/registry

The FileBeat configuration is built based on the variable `filebeat_config`.
For easier management of the contents, the `filebeat_config` variable is made
up of multiple other variables:

* `filebeat_config_prospectors`
* `filebeat_config_output`
* `filebeat_config_shipper`
* `filebeat_config_logging`

```yaml
filebeat_config_prospectors: |
  filebeat:
    prospectors:
      -
        input_type: log
        paths:
          - /var/log/*.log
        registry_file: "{{filebeat_config_registry_file}}"
filebeat_config_output: |
  output:
    elasticsearch:
      hosts: [ 'localhost:9200' ]
filebeat_config_shipper: |
  shipper:
filebeat_config_logging: |
  logging:
    files:
      rotateeverybytes: 10485760 # = 10MB
filebeat_config: |
  {{filebeat_config_prospectors}}
  {{filebeat_config_output}}
  {{filebeat_config_shipper}}
  {{filebeat_config_logging}}
```

## Usage
    - hosts: logging
      roles:
        - { role: torian.filebeat }

## License
Copyright 2016 Emiliano Castagnari

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.

## Author Information
This role was created in 2016 by Emiliano Castagnari.
