# Ansible: mongodb

Configure the components of a MongoDB Cluster

Available on Ansible Galaxy: [pgkehle.mongodb](https://galaxy.ansible.com/pgkehle/mongodb)

## Variables

Required definitions are as follows:

```
cfg_server_name:        "app-cfg"           # (Required)   
cfg_server_group_id:    "app-cfg-servers"   # (Required) Always pass in the group id used for the config servers 
replica_set_group_id:   "app-shd00-servers" # (Optional) If initializing a replica set as part of a MongoDB cluster

```

Host Definitions typically contain the following:

#### Query Server:

```yaml
mongodb_type:           "shard"
cluster_role:           "querysvr"
```

#### Config Server:
```yaml
mongodb_type:           "daemon"
cluster_role:           "configsvr"
replica_set_name:       "app-cfg"               # name of the replica set for the config server (prefix of fqdn)
replica_set_group_id:   "app-cfg-servers"       # group name for all servers in the replica set
```

#### Shard Server:
```yaml
mongodb_type:           "daemon"
cluster_role:           "shardsvr"
replica_set_name:       "app-shd00"             # name of the replica set for the shard server (prefix of fqdn)
replica_set_group_id:   "app-shd00-servers"     # group name for all servers in the replica set
```

## Flags and Variables
```yaml
vars:
  flags:
    - init:           # Install mongo packages
    - config_save:    # Basic initialization.  Stop Services, push service/config files, restart services
    - storage_init:   # Clear directories and logs
    - rs_init:        # Initialize the replica set configuration
    - rs_shards:      # Add shard servers (replica sets) to the cluster
    - db_create:      # Do initial db creation
```

## Examples

```yaml
- hosts: all
  vars: 

    app_users:
      - { db_name: "", user: "", pwd: "", roles: ["readWrite", "userAdmin"] } 

  roles:
    - { role: pgkehle.mongodb, flags: ['init'] }        
    - { role: pgkehle.mongodb, flags: ['config_save'] }        
    - { role: pgkehle.mongodb, flags: ['storage_init'] }        
    - { role: pgkehle.mongodb, flags: ['rs_init'] }        
    - { role: pgkehle.mongodb, flags: ['rs_shards'] }        
    - { role: pgkehle.mongodb, flags: ['db_create'] }        
      
```

```bash
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['init']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['config_save']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['storage_init']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['rs_init']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['rs_shards']}"
ansible-playbook playbooks/mongodb.yml -e "{'flags': ['db_create']}"
```

## License

MIT

## Author Information

Paul Kehle  
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))

## For local development testing

```bash
rsync -av ~/code/ansible-mongodb/* ~/.ansible/roles/pgkehle.mongodb
```

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
* [Secure MongoDB With x509](https://www.mongodb.com/blog/post/secure-mongodb-with-x-509-authentication)

* http://dba.stackexchange.com/questions/80859/issues-with-self-signed-certificates-ssl-and-mongodb
* https://blog.cloudandheat.com/index.php/en/2015/04/19/deploy-a-mongodb-3-0-replica-set-with-x-509-authentication-and-self-signed-certificates/

* https://www.digitalocean.com/community/tutorials/how-to-implement-replication-sets-in-mongodb-on-an-ubuntu-vps
* https://www.digitalocean.com/community/tutorials/how-to-create-a-sharded-cluster-in-mongodb-using-an-ubuntu-12-04-vps
* http://codingmiles.com/mongodb-sharded-cluster-deployment/
* http://dba.stackexchange.com/questions/80859/issues-with-self-signed-certificates-ssl-and-mongodb
* http://demarcsek92.blogspot.com/2014/05/mongodb-ssl-setup.html

