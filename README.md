The migrate.yml playbook can be used to migrate an environment from the `openvswitch` firewall driver to the `iptables_firewall` firewall driver in an OpenStack Newton OSA-based cloud.

To execute:

1. Copy migrate.yml to /opt/openstack-ansible/playbooks
1a. Make any changes to vars for mysql bits
2. Stop all running instances on the given host
3. Execute the following: openstack-ansible migrate.yml --limit <host>
4. Start instances on the given host
