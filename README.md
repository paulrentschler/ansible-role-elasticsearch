paulrentschler.elasticsearch
============================

[![MIT licensed][mit-badge]][mit-link]

Installs and configures ElasticSearch on Ubuntu Linux


Requirements
------------

None.


Role Variables
--------------

The following variables are available with defaults defined in `defaults/main.yml`:

Define the user and group to run ElasticSearch.

    elasticsearch_user: elasticsearch
    elasticsearch_group: elasticsearch


Specify where the defaults file is stored.

    elasticsearch_defaults_file: "/etc/default/elasticsearch"


The "cluster name" to identify the cluster to the nodes for auto-discovery.

    elasticsearch_cluster_name: elasticsearch


Name and settings for this node/host. `primary: true` means that this node/host can become the primary one. `data: true` means that this node/host can store data.

```yaml
elasticsearch_node:
  name: "{{ inventory_hostname }}"
  primary: true
  data: true
  mandatory_plugins: ""
```

Define the network information for this node/host.

```yaml
elasticsearch_network:
  host: "{{ ansible_eth0.ipv4.address }}"
  http_host: "{{ ansible_lo.ipv4.address }}"
  unicast_interface: 'ansible_eth0'
```


Specify the Ansible group of hosts running ElasticSearch. This is used to specify all the nodes/hosts in the cluster.

    elasticsearch_discovery_ansible_group: 'elasticsearch'


Define the settings for indexes.

Note: for development it makes sense to "disable" the distributed features:

- shards: 1
- replicas: 0

These settings directly affect the performance of index and search operations in your cluster. Assuming you have enough machines to hold shards and replicas, the rule of thumb is:

1. Having more *shards* enhances the _indexing_ performance and allows to _distribute_ a big index across machines.
1. Having more *replicas* enhances the _search_ performance and improves the cluster _availability_.

The "shards" is a one-time setting for an index.

The "replicas" can be increased or decreased anytime, by using the Index Update Settings API.

ElasticSearch takes care of load balancing, relocating, gathering the results from nodes, etc. Experiment with different settings to fine-tune your setup.

```yaml
elasticsearch_index:
  shards: 5
  replicas: 1
```


ElasticSearch performs poorly when JVM starts swapping, you should ensure that it _never_ swaps.

Set this property to true to lock the memory (false by default)

    elasticsearch_mlockall: false


Dependencies
------------

None.


Example Playbook
----------------

Minimal example:

```yaml
---
- hosts: all
  roles:
     - paulrentschler.elasticsearch
```

More complex example that includes specifying ...

```yaml
---
- hosts: all
  roles:
    - role: paulrentschler.elasticsearch
      vars:
```


License
-------

[MIT][mit-link]


Author Information
------------------

Created by Paul Rentschler in 2021.


[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://github.com/paulrentschler/ansible-role-elasticsearch/blob/master/LICENSE
