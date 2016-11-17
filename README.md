# Ansible: mongodb-config-cluster

Configure the components of a MongoDB Cluster

Available on Ansible Galaxy: [pgkehle.mongodb-config-cluster](https://galaxy.ansible.com/pgkehle/mongodb-config-cluster)

## Variables

Group definitions are expected as such:

```
[app-query-servers]
app-q0
app-q1

[app-cfg-servers]
app-cfg-rs0
app-cfg-rs1
app-cfg-rs2

[app-shd00-servers]
app-shd00-rs0
app-shd00-rs1
app-shd00-rs2

[app-shd01-servers]
app-shd01-rs0
app-shd01-rs1
app-shd01-rs2

[app-shard-servers:children]
app-shd00-servers
app-shd01-servers

[app-servers:children]
app-query-servers
app-cfg-servers
app-shard-servers
```

Host Definitions typically contain the following:

#### Query Server:

```
mongodb_type:     "shard"
cluster_role:     "querysvr"
first_member:     false
```

#### Config Server:
```
mongodb_type:     "daemon"
cluster_role:     "configsvr"
replica_set_name: "app-cfg"     # name of the replica set for the config server (prefix of fqdn)
first_member:     false
```

#### Shard Server:
```
mongodb_type:     "daemon"
cluster_role:     "shardsvr"
replica_set_name: "app-shd00"   # name of the replica set for the shard server (prefix of fqdn)
first_member:     false
```

#### App Server Config

Also need some helper definitions, included for each server

```
  # name of config server replica set name when configuring query servers
  - cfg_server_name: "app-cfg"  
 
  # name given to the replica set that this server is a part of
  - shard_server_replica_sets:
    - "app-shd00"
    - "app-shd01"
```



# Examples

```YAML

  - hosts: all
 
    # see defaults/main.yml
    vars: 
        # Basic initialization.  Stop Services, push service/config files, clear directories and logs, restart services
        do_config:              true    

        # Initialize the replica set configuration
        do_replica_set_init:    false

        # Add shard servers (replica sets) to the cluster
        do_replica_set_shards:  false

 
    roles:
      - pgkehle.mongodb-config-cluster
```

## License

MIT

## Author Information

Paul Kehle  
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))
