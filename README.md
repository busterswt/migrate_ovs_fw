# migrate_ovs_fw

The `ovsfw-migrate.yml` playbook can be used to migrate an environment from the `openvswitch` firewall driver to the `iptables_firewall` firewall driver in an OpenStack Newton OSA-based cloud. Likewise, the `ovs-revert.yml` playbook can be used to rollback the change (with caveats - see below).

## What's needed

We expect the following:

1. An openrc file exists on every compute node in /root/openrc
2. Every compute node has connectivity to the OpenStack VIP (including Galera)
3. Every compute node has a venv with the neutron/nova/openstack client(s) available

## To execute:

1. Copy the needed playbooks to /opt/openstack-ansible/playbooks

2. Stop running instances on the given host manually or with the following playbook command:

```
openstack-ansible instance-shutdown.yml --limit <host>
```

3. Perform the OpenStack DB and port munging with the following: 

```
openstack-ansible ovsfw-migrate.yml --limit <host>
```

4. Start shutdown instances on the given host manually or with the following playbook command:

```
openstack-ansible instance-startup.yml --limit <host>
```

## To rollback:

0. Be sure this is what you want to do and be aware of the caveats!

1. Stop running instances on the given host manually or with the following playbook command:

```
openstack-ansible instance-shutdown.yml --limit <host>
```

2. Rollback the OpenStack DB and port munging with the following:

```
openstack-ansible ovsfw-revert.yml --limit <host>
```

3. Nova may create qvb/qvo/qbr interfaces upon restart. If so, re-run the command with the following: 

```
openstack-ansible ovsfw-revert.yml --extra-vars "force_delete=true" --limit <host>
```

4. Start shutdown instances on the given host manually or with the following playbook command:

```
openstack-ansible instance-startup.yml --limit <host>
```

### Caveats

- Nova may still recreate the qvo/qvb/qbr interfaces if instances are not started fast enough. As a result, the tap interface in br-int may not get tagged. Start up all instances, re-run the `ovsfw-revert.yml` playbook with `--extra-vars "force_delete=true"`, then restart the `nova-compute` service.
- The `instance-shutdown.yml` playbook writes ACTIVE instances to file. If the play is re-run, it's likely the file will get overwritten and not all instances will be brought back up appropriately

