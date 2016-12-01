# Ansible: mongodb-config

Configure the components of a MongoDB Cluster

Available on Ansible Galaxy: [pgkehle.mongodb-config](https://galaxy.ansible.com/pgkehle/mongodb-config)

## Variables

Required definitions are as follows:

```
cfg_server_name:        "app-cfg"           # (Required)   
cfg_server_group_id:    "app-cfg-servers"   # (Required) Always pass in the group id used for the config servers 
replica_set_group_id:   "app-shd00-servers" # (Optional) If initializing a replica set as part of a MongoDB cluster

```

Host Definitions typically contain the following:

#### Query Server:

```
mongodb_type:           "shard"
cluster_role:           "querysvr"
first_member:           false
```

#### Config Server:
```
mongodb_type:           "daemon"
cluster_role:           "configsvr"
replica_set_name:       "app-cfg"               # name of the replica set for the config server (prefix of fqdn)
replica_set_group_id:   "app-cfg-servers"       # group name for all servers in the replica set
first_member:           false
```

#### Shard Server:
```
mongodb_type:           "daemon"
cluster_role:           "shardsvr"
replica_set_name:       "app-shd00"             # name of the replica set for the shard server (prefix of fqdn)
replica_set_group_id:   "app-shd00-servers"     # group name for all servers in the replica set
first_member:           false
```

## Variables

Flags for which sections to run
```
pkg_install:    Install mongo packages
config_save:    Basic initialization.  Stop Services, push service/config files, clear directories and logs, restart services
rs_init:        Initialize the replica set configuration
rs_shards:      Add shard servers (replica sets) to the cluster
```

## Examples

```YAML

  - hosts: all
 
    # see defaults/main.yml
    vars: 

 
    roles:
      - { name: pgkehle.mongodb-config, pkg_install: true }
      - { name: pgkehle.mongodb-config, config_save: true }
      - { name: pgkehle.mongodb-config, rs_init: true }
      - { name: pgkehle.mongodb-config, rs_shards: true }
      - { name: pgkehle.mongodb-config, db_create: true }
```

## License

MIT

## Author Information

Paul Kehle  
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))

### References

* [Security Hardening](https://docs.mongodb.com/manual/core/security-hardening/)
* [X509](https://docs.mongodb.com/manual/core/security-x.509/)
* [Deploy Shard Cluster](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/)
* [Add Shards to Cluster](https://docs.mongodb.com/v3.2/tutorial/add-shards-to-shard-cluster/)
* [Authorization](https://docs.mongodb.com/manual/core/authorization/)
* [Internal Auth](https://docs.mongodb.com/manual/core/security-internal-authentication/)
* [Configure Member Certificates](https://docs.mongodb.com/manual/tutorial/configure-x509-member-authentication/*x509-member-certificate)
* [Enforce Keyfile Access Control](https://docs.mongodb.com/manual/tutorial/enforce-keyfile-access-control-in-existing-replica-set/)
* [Deploy Replica Set w/ Keyfill Access Control](https://docs.mongodb.com/v3.2/tutorial/deploy-replica-set-with-keyfile-access-control/)
* [db.createUser()](https://docs.mongodb.com/manual/reference/method/db.createUser/#db.createUser)

* http://dba.stackexchange.com/questions/80859/issues-with-self-signed-certificates-ssl-and-mongodb
* https://blog.cloudandheat.com/index.php/en/2015/04/19/deploy-a-mongodb-3-0-replica-set-with-x-509-authentication-and-self-signed-certificates/

* https://www.digitalocean.com/community/tutorials/how-to-implement-replication-sets-in-mongodb-on-an-ubuntu-vps
* https://www.digitalocean.com/community/tutorials/how-to-create-a-sharded-cluster-in-mongodb-using-an-ubuntu-12-04-vps
* http://codingmiles.com/mongodb-sharded-cluster-deployment/
* http://dba.stackexchange.com/questions/80859/issues-with-self-signed-certificates-ssl-and-mongodb
* http://demarcsek92.blogspot.com/2014/05/mongodb-ssl-setup.html

