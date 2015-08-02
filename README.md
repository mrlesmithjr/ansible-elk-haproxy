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
config_hosts_file: false  #defines if /etc/hosts should include ELK hosts...if DNS not configured...Vagrant testing.
config_logstash: false
enable_haproxy_admin_page: true
haproxy_admin_password: admin  #defines password for admin user to login to admin page
haproxy_admin_port: 9090  #defines http port to listen on for admin page
haproxy_admin_user: admin  #defines admin user to login to admin page
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
mrlesmithjr.keepalived
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
