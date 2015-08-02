Role Name
=========

Installs ELK Stack Role (ELK-HAProxy)

Requirements
------------

Prior to using this role you will want to add your nodes to the appropriate inventory group and create the corresponding group_vars/group with variables defined. You should create 2 elk-haproxy nodes. Examples below.
#####hosts inventory
````
[elk-nodes]
elk-haproxy-1
elk-haproxy-2

[elk-haproxy-nodes]
elk-haproxy-1
elk-haproxy-2
````

Role Variables
--------------

````
clear_logstash_config: false
config_logstash: false
enable_haproxy_admin_page: true
haproxy_admin_password: admin  #defines password for admin user to login to admin page
haproxy_admin_port: 9090  #defines http port to listen on for admin page
haproxy_admin_user: admin  #defines admin user to login to admin page
haproxy_configs:
  - name: logstash-syslog
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 514
    port_fe: 514
    proto: tcp
  - name: logstash-vCenter
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 1515
    port_fe: 1515
    proto: tcp
  - name: logstash-Netscaler
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 1517
    port_fe: 1517
    proto: tcp
  - name: logstash-Windows-Eventlog
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 3515
    port_fe: 3515
    proto: tcp
  - name: logstash-Windows-IIS
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 3525
    port_fe: 3525
    proto: tcp
  - name: logstash-Redis
    balance: leastconn
    elk_group_name: elk-broker-nodes
    port_be: 6379
    port_fe: 6379
    proto: tcp
  - name: elasticsearch
    balance: leastconn
    elk_group_name: elk-processor-nodes
    port_be: 9200
    port_fe: 9200
    proto: tcp
  - name: elasticsearch
    balance: leastconn
    elk_group_name: elk-broker-nodes
    port_be: 9300
    port_fe: 9300
    proto: tcp
  - name: logstash-Rundeck
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: '{{ rundeck_logstash_port }}'
    port_fe: '{{ rundeck_logstash_port }}'
    proto: tcp
  - name: logstash-Syslog
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_fe: 10514
    port_fe: 10514
    proto: tcp
  - name: elasticsearch-Curator
    balance: leastconn
    elk_group_name: elk-pre-processor-nodes
    port_be: 28778
    port_fe: 28778
    proto: tcp
  - name: logstash-Kibana
    balance: leastconn
    elk_group_name: elk-broker-nodes
    port_be: '{{ kibana_port }}'
    port_fe: 80
  - name: logstash-Kibana
    balance: leastconn
    elk_group_name: elk-broker-nodes
    port_be: '{{ kibana_port }}'
    port_fe: '{{ kibana_port }}'
kibana_port: 5601
logstash_config_dir: /etc/logstash/conf.d
logstash_configs:
  - 000_inputs
#  - 001_filters
  - 999_outputs
logstash_file_inputs:
  - path: /var/log/nginx/access.log
    type: nginx-access
  - path: /var/log/nginx/error.log
    type: nginx-error
  - path: /var/log/mail.log
    type: postfix-log
  - path: /var/log/redis/redis-server.log
    type: redis-server
logstash_inputs: []
#  - prot: tcp
#    codec: json
#    port: '{{ rundeck_logstash_port }}'
#    type: rundeck
#  - prot: udp
#    buffer_size: 1452
#    codec: collectd
#    port: 25826
#    type: collectd
logstash_log_dir: /var/log/logstash
logstash_outputs:
  - output: redis
    output_host: '{{ logstash_server_fqdn }}'
reset_logstash_config: false
rundeck_logstash_port: 9700
````

Dependencies
------------

````
mrlesmithjr.ntp
mrlesmithjr.rsyslog
mrlesmithjr.snmpd
mrlesmithjr.timezone
mrlesmithjr.logstash
mrlesmithjr.haproxy
````

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

- hosts: elk-haproxy-nodes
  roles:
     - { role: mrlesmithjr.elk-haproxy }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
