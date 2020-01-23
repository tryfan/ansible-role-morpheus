- [Morpheus](#morpheus)
  * [Description](#description)
  * [Role Variables](#role-variables)
  * [Example Playbook](#example-playbook)
  * [License](#license)
  * [Author Information](#author-information)

# Morpheus

Installation of Morpheus

## Description

This role will deploy Morpheus onto an EL or Ubuntu based platform.
The only required variable for deploying a single appliance is `appliance_url`

## Role Variables


|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|`appliance_url`|Y|`https://morpheus.example.com`|URL of the appliance/load balancer|
|`morpheus_package_centos`|N|`https://downloads.morpheusdata.com/files/morpheus-appliance-4.1.2-1.el7.x86_64.rpm`|Morpheus Appliance RPM|
|`morpheus_offline_package_centos`|N|`https://downloads.morpheusdata.com/files/morpheus-appliance-offline-4.1.2-1.noarch.rpm`|Morpheus Offline RPM; This package contains packages that the Morpheus installer would otherwise pull down.|
|`morpheus_package_ubuntu`|N|`https://downloads.morpheusdata.com/files/morpheus-appliance_4.1.2-1_amd64.deb`|Morpheus Appliance DEB|
|`morpheus_offline_package_ubuntu`|N|`https://downloads.morpheusdata.com/files/morpheus-appliance-offline_4.1.2-1_all.deb`|Morpheus Offline DEB; This package contains packages that the Morpheus installer would otherwise pull down.|
|`morpheus_group`|Y|`morpheus`|Inventory group name for the Morpheus UI node(s)|
|`morpheus_rabbitmq_group`|N|`rabbitmq`|Inventory group name for the RabbitMQ node(s) - Only required when deploying HA and running RabbitMQ on Morpheus UI nodes|
|`morpheus_db_group`|N|`db`|Inventory group name for the database node(s)|
|`initial_config`|N|`false`|Initial configuration flag.  If set to true and run against a morpheus group, it will reconfigure the group, regardless of morpheus-secrets.json existence.|
|`morpheus_rabbitmq_external_cookie`|N|`""`|Custom Erlang cookie for rabbitmq if not running the ansible-role-rabbitmq-cluster role.|
|`morpheus_elastic_tls`|N|`false`|Use TLS with Elasticsearch|
|`morpheus_elastic_cluster_name`|N|`morpheus`|Elasticsearch cluster name, only used for external ES|
|`morpheus_mysql_external`|N|`false`|Use external MySQL DB|
|`morpheus_rabbitmq_external`|N|`false`|Use external RabbitMQ|
|`morpheus_elastic_external`|N|`false`|Use external Elasticsearch|
|`morpheus_elastic_hosts`|N|`{}`|List of dictionaries describing Elasticsearch hosts.  Args: `host`, `port`|
|`morpheus_rabbitmq_lb`|N|`127.0.0.1`|RabbitMQ load balancer.  If using RabbitMQ on UI nodes, this uses it's local cluster member to communicate with the cluster.|
|`morpheus_db`|N|`morpheusdb`|Morpheus database name|
|`morpheus_db_user`|N|`morpheus`|Morpheus database user|
|`morpheus_db_pass`|N|`Pa55w0rd!`|Morpheus database password|
|`morpheus_db_port`|N|`3306`|Morpheus database port|
|`morpheus_db_url_override`|N|`false`|Enable Morpheus database URL override for JDBC connection strings and options.| 
|`morpheus_db_url_override_options`|N|`""`|If JDBC override is required, this is the strong on the end of the URL.|
<!-- TODO: Make sure to include a section for external DB to explain the url override -->


## All in One Appliance

An all in one Morpheus appliance uses embedded RabbitMQ, MariaDB, and Elasticsearch.

### Required Variables

`appliance_url`

### Example Playbook

    - hosts: morpheus
      gather_facts: true
      become: true
      vars:
        appliance_url: "https://morpheus.home.localdomain"
      roles:
        - ansible-role-morpheus

## External Database

If you have an external MySQL 5.7 compatible database to use, you can specify the connection parameters for Morpheus to use.

### Required Variables

`morpheus_mysql_external`

`morpheus_db_group`

`morpheus_db`

`morpheus_db_user`

`morpheus_db_pass`

## License

MIT

## Author Information

Nick Celebic
https://github.com/ncelebic