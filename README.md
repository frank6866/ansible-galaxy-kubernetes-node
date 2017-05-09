Kubernetes Node
===============
This role is used to deploy kubernetes node.

Inventory file demo
-------------------

```
k8s-5 ansible_ssh_host=10.10.10.15 ansible_ssh_port=22 ansible_ssh_user=centos


[kubernetes-node]
k8s-5

```

Role Variables
--------------

The whole example of group_vas/kubernetes-node.yml file as follows:

```
ip_hostname_in_etc_hosts:
  - ip: 10.10.10.15
    hostname: k8s-5
    state: present

kube_node_master_ip: 10.10.10.11

```

The Kubernetes cluster need to visit each other by hostname, if the hosts is not registered in DNS server, you can set **ip_hostname_in_etc_hosts** variable to add the id and hostname pair in /etc/hosts file.


Example Playbook
----------------

```
- hosts: kubernetes-node
  become: true
  roles:
    - frank6866.linux_basic
    - frank6866.kubernetes-node
```


License
-------

MIT

