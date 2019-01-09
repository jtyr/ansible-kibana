kibana
======

Ansible role which helps to install and configure Kibana.

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name: Example of how to install Kibana with the default configuration
  hosts: all
  roles:
    - kibana

- name: Example of how to customize the configuration
  hosts: all
  vars:
    # Bind to all network interfaces
    kibana_config_server_host: 0.0.0.0
    # Change the default port
    kibana_config_server_port: 8080
    # Change the Elasticsearch URL
    kibana_config_elasticsearch_url: http://myserver:9200
    # Add custom configuration
    kibana_config__custom:
      # Enable verbose logging
      logging:
        verbose: yes
    # Install plugins
    kibana_plugins:
      - x-pack
  roles:
    - kibana
```


Role variables
--------------

```yaml
# Package to be installed (explicit version can be specified here)
kibana_pkg: kibana

# YUM repo URL
kibana_yum_repo_url: "{{ elastic_yum_repo_url | default('https://artifacts.elastic.co/packages/6.x/yum') }}"

# YUM repo GPG key
kibana_yum_repo_key: "{{ elastic_yum_repo_key | default('https://artifacts.elastic.co/GPG-KEY-elasticsearch') }}"

# Extra YUM repo params
kibana_yum_repo_params: "{{ elastic_yum_repo_params | default({}) }}"

# GPG key for the APT repo
kibana_apt_repo_key: "{{ elastic_apt_repo_key | default('https://artifacts.elastic.co/GPG-KEY-elasticsearch') }}"

# APT repo string
kibana_apt_repo_string: "{{ elastic_apt_repo_string | default('deb https://artifacts.elastic.co/packages/6.x/apt stable main') }}"

# Extra APT repo params
kibana_apt_repo_params: "{{ elastic_apt_repo_params | default({}) }}"

# Name of the service
kibana_service: kibana


# Path to the kibana-plugin executable
kibana_plugins_bin: /usr/share/kibana/bin/kibana-plugin

# List of plugins to install
kibana_plugins: []


# Path to the Kibana config file
kibana_config_file: /etc/kibana/kibana.yml

# Values of default configurations
kibana_config_elasticsearch_url: http://localhost:9200

# Default options of the elasticsearch section
kibana_config_elasticsearch__default:
  url: "{{ kibana_config_elasticsearch_url }}"

# Default options of the elasticsearch section
kibana_config_elasticsearch__custom: {}

# Final options of the elasticsearch section
kibana_config_elasticsearch: "{{
  kibana_config_elasticsearch__default | combine(
  kibana_config_elasticsearch__custom) }}"


# Values of the default options of the server section
kibana_config_server_host: 127.0.0.1
kibana_config_server_port: 5601

# Default options of the server section
kibana_config_server__default:
  host: "{{ kibana_config_server_host }}"
  port: "{{ kibana_config_server_port }}"

# Custom options of the server section
kibana_config_server__custom: {}

# Final options of the server section
kibana_config_server: "{{
  kibana_config_server__default | combine(
  kibana_config_server__custom) }}"


# Default configuration
kibana_config__default:
  elasticsearch: "{{ kibana_config_elasticsearch }}"
  server: "{{ kibana_config_server }}"

# Custom configuration
kibana_config__custom: {}

# Final configuration
kibana_config: "{{
  kibana_config__default | combine(
  kibana_config__custom) }}"
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
