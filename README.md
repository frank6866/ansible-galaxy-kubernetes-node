mariadb galear
=====================

Install mariadb galera.


Inventory file demo
-------------------

```
mariadb-1 ansible_ssh_host=10.10.10.5 ansible_ssh_port=22 ansible_ssh_user=root ip=10.10.10.5 role="init_cluster_node"
mariadb-2 ansible_ssh_host=10.10.10.6 ansible_ssh_port=22 ansible_ssh_user=root ip=10.10.10.6
mariadb-3 ansible_ssh_host=10.10.10.7 ansible_ssh_port=22 ansible_ssh_user=root ip=10.10.10.7


[cluster1]
mariadb-[1:3]

[mariadb_galera:children]
cluster1

[mariadb_galera:vars]
# add_ip_hostname_in_etc_hosts=true

```

* the **ip** variable is used for **wsrep_node_address** item in /etc/my.cnf.d/galera.cnf
* thie **add_ip_hostname_in_etc_hosts** variable is used to config hostname resolving if server is not registered in DNS Server. If your server is registered in DNS Server, you can comment this variable.


Role Variables
--------------
No required variables.

If you want to set the root password for the first time, you can set **mariadb_root_password** variable, this role does not support changing root password after the cluster is up, but you can change them by hands

> mariadb_root_password: change_me

You can change the listen port by setting **mariadb_listen_port** variable:

> mariadb_listen_port: 3306

You can change **mariadb_open_files_limit** and **mariadb_max_connections** by setting the following variable:

```
mariadb_open_files_limit: 2048
mariadb_max_connections: 1024
```

Notice that **mariadb_open_files_limit** must great than **mariadb_max_connections**.

If you want to create database by the script, you can set the **mariadb_databases** variable:

```
mariadb_databases:
  - keystone
  - glance
  - cinder
  - nova_api
  - nova
  - neutron
```

If you want to create user and set privilege, you can set **mariadb_users** variable like this:

```
mariadb_users:
  - name: keystone
    password: change_me
    host: "%"
    privilege: "keystone.*:ALL"
    state: present
  - name: keystone
    password: change_me
    host: "localhost"
    privilege: "keystone.*:ALL"
    state: present
```


Example Playbook
----------------

```
- hosts: mariadb_galera
  become: true
  roles:
    - frank6866.mariadb-galera
```


TODO
-----
1. change the openstack repo to mariadb offical repo
2. install mariadb package by enanbling mariadb repo name


License
-------

MIT

