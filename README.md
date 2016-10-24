kibana
======

Ansible role which helps to install and configure Kibana.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
---

- name: Example of how to install Kibana with the default configuration
  hosts: all
  roles:
    - kibana

- name: Example of how to customize the configuration
  hosts: all
  vars:
    kibana_config:
      # Change the Elasticsearch URL
      elasticsearch:
        url: http://10.0.0.15:9200
  roles:
    - kibana
```


Role variables
--------------

```
# Package to be installed (explicit version can be specified here)
kibana_pkg: kibana

# YUM repo URL
kibana_yum_repo_url: https://packages.elastic.co/kibana/4.6/centos

# YUM repo GPG key
kibana_yum_repo_key: https://packages.elastic.co/GPG-KEY-elasticsearch

# Extra EPEL YUM repo params
kibana_yum_repo_params: {}

# Name of the service
kibana_service: kibana


# Path to the Kibana config file
kibana_config_file: /opt/kibana/config/kibana.yml

# Default configuration
kibana_config__default: {}

# Custom configuration
kibana_config__custom: {}

# Final configuration
kibana_config: "{{
  kibana_config__default.update(
  kibana_config__custom) }}{{
  kibana_config__default }}"
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)
- [`elasticsearch`](https://github.com/jtyr/ansible-elasticsearch) (optional)
- [`filebeat`](https://github.com/jtyr/ansible-filebeat) (optional)
- [`logstash`](https://github.com/jtyr/ansible-logstash) (optional)


License
-------

MIT


Author
------

Jiri Tyr
