# Ansible Role - mongodb

Configure the components of a MongoDB Cluster

Available on Ansible Galaxy: [pgkehle.mongodb](https://galaxy.ansible.com/pgkehle/mongodb)

## Variables

Required definitions are as follows:

```yaml
cfg_server:
  name:                 "my-cfg"              # (Required)
  group:                "my-cfg-servers"      # (Required) Always pass in the group id used for the config servers

replica_set:
  name:                 "my-cfg"              # name of the replica set for the config server (prefix of fqdn)
  group:                "my-cfg-servers"      # group name for all servers in the replica set
```

Host Definitions typically contain the following:

### Router Server

```yaml
cluster_role:           "router"
```

### Config Server

```yaml
cluster_role:           "config"
replica_set:
  name:                 "my-cfg"              # name of the replica set for the config server (prefix of fqdn)
  group:                "my-cfg-servers"      # group name for all servers in the replica set
```

### Shard Server

```yaml
cluster_role:           "shard"
replica_set:
  name:                 "db-data"             # name of the replica set for the shard server (prefix of fqdn)
  group:                "db-data-servers"     # group name for all servers in the replica set
```

## Flags and Variables

```yaml
vars:
  flags:
    - install:              # Install mongo packages
    - save-config:          # Basic initialization.  Stop Services, push service/config files, restart services
    - reset-storage:        # Clear directories and logs
    - init-replica-set:     # Initialize the replica set configuration
    - add-shard-to-cluster: # Add a replica set of a shard server to the cluster of shard servers
    - db_create:            # Do initial db creation
  new_shard:
    name:                   # Name of the replica set to add to the config server
    server:                 # One of the members of the new replicate set to add
```

## Examples

```yaml
- hosts: all
  vars:
    app_users:
      - { db_name: "", user: "", pwd: "", roles: ["readWrite", "userAdmin"] }

  roles:
    - { role: pgkehle.mongodb, flags: ['install'] }
    - { role: pgkehle.mongodb, flags: ['save-config'] }
    - { role: pgkehle.mongodb, flags: ['reset-storage'] }
    - { role: pgkehle.mongodb, flags: ['init-replica-set'] }
    - { role: pgkehle.mongodb, flags: ['add-shard-to-cluster'] }
    - { role: pgkehle.mongodb, flags: ['db_create'] }
```

```bash
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['install']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['save-config']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['reset-storage']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['init-replica-set']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['add-shard-to-cluster']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['db_create']}"
```

## License

MIT

## Author Information

Paul Kehle  
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))

### References

* MongoDB
  * [Security Hardening](https://docs.mongodb.com/manual/core/security-hardening/)
  * [X509](https://docs.mongodb.com/manual/core/security-x.509/)
  * [Deploy Shard Cluster](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/)
  * [Add Shards to Cluster](https://docs.mongodb.com/manual/tutorial/add-shards-to-shard-cluster)
  * [Authorization](https://docs.mongodb.com/manual/core/authorization/)
  * [Internal Auth](https://docs.mongodb.com/manual/core/security-internal-authentication/)
  * [Configure Member Certificates](https://docs.mongodb.com/manual/tutorial/configure-x509-member-authentication/*x509-member-certificate)
  * [Enforce Keyfile Access Control](https://docs.mongodb.com/manual/tutorial/enforce-keyfile-access-control-in-existing-replica-set/)
  * [Deploy Replica Set w/ Keyfill Access Control](https://docs.mongodb.com/v3.2/tutorial/deploy-replica-set-with-keyfile-access-control/)
  * [db.createUser()](https://docs.mongodb.com/manual/reference/method/db.createUser/#db.createUser)
  * [Secure MongoDB With x509](https://www.mongodb.com/blog/post/secure-mongodb-with-x-509-authentication)

* Digital Ocean
  * [Implement Replica Sets on ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-implement-replication-sets-in-mongodb-on-an-ubuntu-vps)
  * [Create a Sharded Clister 12.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-sharded-cluster-in-mongodb-using-an-ubuntu-12-04-vps)
