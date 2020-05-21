# Morpheus

Morpheus Cloud Management Platform

- [Morpheus](#morpheus)
  * [Description](#description)
  * [Supported Versions](#supported-versions)
  * [Examples](#examples)
  * [Role Variables](#role-variables)
    + [RabbitMQ Variables](#rabbitmq-variables)
    + [ElasticSearch Variables](#elasticsearch-variables)
    + [Database Variables](#database-variables)
  * [Morpheus Installation](#morpheus-installation)
    + [All in One Appliance](#all-in-one-appliance)
      - [Required Variables](#required-variables)
      - [Usage](#usage)
    + [Highly Available Appliance](#highly-available-appliance)
      - [Required Variables](#required-variables-1)
      - [Usage](#usage-1)
  * [External Database](#external-database)
    + [Single Master](#single-master)
      - [Required Variables](#required-variables-2)
      - [Usage](#usage-2)
    + [Percona XtraDB Database Cluster](#percona-xtradb-database-cluster)
      - [Required Variables](#required-variables-3)
      - [Usage](#usage-3)
  * [External Elasticsearch](#external-elasticsearch)
    + [Required Variables](#required-variables-4)
    + [Usage](#usage-4)
  * [External RabbitMQ](#external-rabbitmq)
    + [Required Variables](#required-variables-5)
    + [Usage](#usage-5)
  * [License](#license)
  * [Author Information](#author-information)

## Description

This role will deploy Morpheus onto an EL or Ubuntu based platform.
The only required variable for deploying a single appliance is `morpheus_appliance_url`

NOTE: The ansible-role-rabbitmq-cluster role will not work on Ubuntu because of a problem with their PackageCloud repositories.  If installing
on Ubuntu, only the single node appliance is supported with these roles.

## Supported Versions

Both all in one and HA deployments have only been tested against Morpheus 4.2.0.  All in one should work with almost any version, but it has not been tested.

## Examples

Refer to the examples repo here: https://github.com/tryfan/ansible-morpheus-examples

## Role Variables

|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|`morpheus_appliance_url`|Y|`https://morpheus.example.com`|URL of the appliance/load balancer|
|`morpheus_use_custom_repo`|N|`false`|Modify custom repo for Morpheus installation
|`morpheus_package_centos`|N|`in defaults/main.yml`|Morpheus Appliance RPM|
|`morpheus_offline_package_centos`|N|`in defaults/main.yml`|Morpheus Offline RPM; This package contains packages that the Morpheus installer would otherwise pull down.|
|`morpheus_package_ubuntu`|N|`in defaults/main.yml`|Morpheus Appliance DEB|
|`morpheus_offline_package_ubuntu`|N|`in defaults/main.yml`|Morpheus Offline DEB; This package contains packages that the Morpheus installer would otherwise pull down.|
|`morpheus_use_offline_package`|N|`false`|Whether to download and install the offline package for installation|
|`morpheus_group`|Y|`morpheus`|Inventory group name for the Morpheus UI node(s)|
|`morpheus_upgrade_flag`|N|`false`|When running the playbook, an RPM is downloaded by default.  Setting this flag forces its installation|
|`morpheus_package_state`|N|`present`|Default package state.  Modified later if `morpheus_upgrade_flag` is set|
|`morpheus_down_string`|N|`ok: down: morpheus-ui`|Morpheus UI down string|
|`morpheus_check_down_string`|N|`ok: down: morpheus-ui`|Morpheus UI check_down string|
|`morpheus_setup`|Y|`false`|Flag for initial appliance setup after installation| 
|`morpheus_setup_appliance_name`|N|`MorpheusTest`|Name of your appliance|
|`morpheus_setup_appliance_url`|N|`{{ morpheus_appliance_url }}`|URL of the appliance/load balancer|
|`morpheus_setup_master_tenant`|N|`MasterTenant`|Master Tenant Name|
|`morpheus_setup_admin_username`|N|`admin`|Admin username|
|`morpheus_setup_admin_password`|N|`morphPass1@`|Admin password|
|`morpheus_setup_admin_email`|N|`test@example.com`|Admin email|
|`morpheus_setup_admin_firstname`|N|`admin`|Admin first name|
|`morpheus_setup_admin_lastname`|N|`admin`|Admin last name|
|`morpheus_setup_enable_backups`|N|`false`|Flag to enable backups|
|`morpheus_setup_enable_monitoring`|N|`false`|Flag to enable monitoring|
|`morpheus_setup_enable_logs`|N|`false`|Flag to enable logs|
|`morpheus_setup_hub_mode`|N|`skip`|Morpheus Hub activation: possible values are `skip`, `login`, and `register`|
|`morpheus_setup_hub_email`|N|`""`|Morpheus Hub email address|
|`morpheus_setup_hub_password`|N|`""`|Morpheus Hub password|
|`morpheus_setup_hub_firstname`|N|`""`|Morpheus Hub first name|
|`morpheus_setup_hub_lastname`|N|`""`|Morpheus Hub last name|
|`morpheus_setup_hub_jobtitle`|N|`""`|Morpheus Hub job title|
|`morpheus_setup_hub_companyname`|N|`""`|Morpheus Hub company name|

### RabbitMQ Variables

|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|`morpheus_rabbitmq_external`|N|`false`|Use external RabbitMQ|
|`morpheus_rabbitmq_lb`|N|`127.0.0.1`|RabbitMQ load balancer.  If using RabbitMQ on UI nodes, this uses it's local cluster member to communicate with the cluster.|
|`morpheus_rabbitmq_user`|N|`morpheus`|RabbitMQ User|
|`morpheus_rabbitmq_password`|N|`rabbitmqmorpheuspass`|RabbitMQ Password|
|`morpheus_rabbitmq_vhost`|N|`morpheus`|RabbitMQ vhost|
|`morpheus_rabbitmq_port`|N|`5672`|RabbitMQ Port|
|`morpheus_rabbitmq_stomp_port`|N|`61613`|RabbitMQ Stomp Port|
|`morpheus_rabbitmq_heartbeat`|N|`50`|RabbitMQ Heartbeat timeout|
|`morpheus_rabbitmq_use_tls`|N|`false`|Enable RabbitMQ TLS|
|`morpheus_rabbitmq_external_cookie`|N|`""`|Custom Erlang cookie for rabbitmq if not running the ansible-role-rabbitmq-cluster role.|

### ElasticSearch Variables

|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|`morpheus_elastic_external`|N|`false`|Use external Elasticsearch|
|`morpheus_elastic_hosts`|N|`{}`|List of dictionaries describing Elasticsearch hosts.  Args: `host`, `port`|
|`morpheus_elastic_tls`|N|`false`|Use TLS with Elasticsearch|
|`morpheus_elastic_cluster_name`|N|`morpheus`|Elasticsearch cluster name, only used for external ES|
|`morpheus_elastic_auth_user`|N|`""`|User for Elasticsearch, if required|
|`morpheus_elastic_auth_password`|N|`""`|Password for Elasticsearch, if required|
|`morpheus_elastic_ca_file`|N|`""`|Elasticsearch CA file, if required|

### Database Variables

|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|`morpheus_mysql_external`|N|`false`|Use external MySQL compatible DB and do not install embedded MySQL|
|`morpheus_db_group`|N|`db`|Inventory group name for the database node(s)|
|`morpheus_db`|N|`morpheusdb`|Morpheus database name|
|`morpheus_db_user`|N|`morpheus`|Morpheus database user|
|`morpheus_db_pass`|N|`Pa55w0rd!`|Morpheus database password|
|`morpheus_db_port`|N|`3306`|Morpheus database port|
|`morpheus_db_url_override`|N|`false`|Enable Morpheus database URL override for JDBC connection strings and options.|
|`morpheus_db_url_override_options`|N|`""`|If JDBC override is required, this is the strong on the end of the URL.|
<!-- TODO: Make sure to include a section for external DB to explain the url override -->

## Morpheus Installation

This role downloads and installs the Morpheus Cloud Management Platform

### All in One Appliance

An all in one Morpheus appliance uses embedded RabbitMQ, MariaDB, and Elasticsearch.

#### Required Variables

- `morpheus_appliance_url`

#### Usage

Set `morpheus_appliance_url` to the DNS name of your appliance.  This can be a CNAME.

### Highly Available Appliance

This role can install Morpheus as a highly available set.

NOTE: An external MySQL compatible database is required to run multiple appliances.

#### Required Variables

- `morpheus_appliance_url`
- `morpheus_mysql_external` - see External Database below for more details

#### Usage

Set `morpheus_appliance_url` to the load balancer DNS name that will point to your appliance hosts.

Put your appliance hosts in the inventory under the `morpheus_group` group.

### Morpheus Appliance Initial Setup

If `morpheus_setup` is true, this role will initially setup your appliance.  

## External Database

### Single Master

If you have an external MySQL 5.7 compatible database to use, you can specify the connection parameters for Morpheus to use.

#### Required Variables

- `morpheus_mysql_external`
- `morpheus_db_group`
- `morpheus_db`
- `morpheus_db_user`
- `morpheus_db_pass`

#### Usage

Setting `morpheus_mysql_external` to true disables initial installation of the embedded MySQL package in Morpheus.

Define your external database master in the `morpheus_db_group` group in the inventory.  This will get its facts and define the configuration appropriately.  

For database creation, create `morpheus_db_user` with `morpheus_db_pass`, create `morpheus_db` and give the user the following MySQL privileges.

- `*.*:SELECT,PROCESS,SHOW DATABASES/{{morpheus_db}}.*:ALL`

When the initial Morpheus reconfigure is run, Morpheus will create the table structure.  For additional details, visit <https://docs.morpheusdata.com>

### Percona XtraDB Database Cluster

This role enables you to use a Percona XtraDB cluster.

#### Required Variables

- `morpheus_mysql_external`
- `morpheus_db_group`
- `morpheus_db`
- `morpheus_db_user`
- `morpheus_db_pass`

#### Usage

Set `morpheus_mysql_external` to true to disable the embedded MySQL.  

Define all the database cluster members in the inventory as part of the `morpheus_db_group` group.

Set `morpheus_db_url_override` to true to enable usage of a JDBC string in the Morpheus configuration.
For percona, we recommend the following string in `morpheus_db_url_override_options`:

- `"/{{ morpheus_db }}?autoReconnect=true&useUnicode=true&characterEncoding=utf8&failOverReadOnly=false&useSSL=false"`

If you need to install a Percona clustered database, take a look at <https://github.com/ncelebic/ansible-role-XtraDB-Cluster>

## External Elasticsearch

This role enables you to use an external Elasticsearch cluster.

NOTE: See <https://docs.morpheusdata.com> for the version requirements for Elasticsearch

### Required Variables

- `morpheus_elastic_external`
- `morpheus_elastic_cluster_name`
- `morpheus_elastic_tls`
- `morpheus_elastic_hosts`

### Usage

Set `morpheus_elastic_external` to true to disable the embedded Elasticsearch and set the name in `morpheus_elastic_cluster_name`.

If using TLS, set `morpheus_elastic_tls` to true.

Define all Elasticsearch hosts and ports in `morpheus_elastic_hosts`. eg.

    morpheus_elastic_hosts:
      - host: "myelastichost.example.com"
        port: 443

## External RabbitMQ

This role enables you to use an external RabbitMQ cluster.

### Required Variables

- `morpheus_rabbitmq_external`
- `morpheus_rabbitmq_lb`
- `morpheus_rabbitmq_user`
- `morpheus_rabbitmq_password`
- `morpheus_rabbitmq_vhost`

### Usage 

Set `morpheus_rabbit_external` to true to disable the embedded RabbitMQ.

Set `morpheus_rabbitmq_user` and `morpheus_rabbitmq_password` to the credentials that have access to the `morpheus_rabbitmq_vhost` vhost in RabbitMQ.

The vhost queues must have certain policies on them for Morpheus, see <https://docs.morpheusdata.com/en/4.1.2/getting_started/installation/distributed/full/rabbitmq.html#rabbitmq-cluster> for details.

The role <https://github.com/ncelebic/ansible-role-rabbitmq-cluster> can set up a RabbitMQ cluster and set the vhost and policies for you.

If desired, you can run RabbitMQ externally on the same hosts as the Application/UI nodes.  If you do, set the `morpheus_rabbitmq_lb` to 127.0.0.1 and the UI nodes will use their own local clustered node for queue communication.

## License

MIT

## Author Information

Nick Celebic
<https://github.com/ncelebic>
