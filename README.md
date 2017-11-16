# migrate_ovs_fw

The migrate.yml playbook can be used to migrate an environment from the `openvswitch` firewall driver to the `iptables_firewall` firewall driver in an OpenStack Newton OSA-based cloud.

## What's implied

We expect the following:

1. An openrc file exists on every compute node in /root/openrc
2. Every compute node has connectivity to the OpenStack VIP (including Galera)
3. Every compute node has a venv with the neutron client available

## To execute:

1. Copy migrate.yml to /opt/openstack-ansible/playbooks

2. Stop all running instances on the given host

3. Execute the following: 

```
openstack-ansible migrate.yml --limit <host>
```

4. Start instances on the given host

## To rollback:

1. Stop all running instances on the given host

2. Execute the following: 

```
openstack-ansible revert.yml --limit <host>
```

3. Nova may create qvb/qvo/qbr interfaces upon restart. If so, re-run the command with the following: 

```
openstack-ansible revert.yml --extra-vars "force_delete=true" --limit <host>
```

4. Start instances on the given host
