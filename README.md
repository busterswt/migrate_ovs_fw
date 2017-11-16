The migrate.yml playbook can be used to migrate an environment from the `openvswitch` firewall driver to the `iptables_firewall` firewall driver in an OpenStack Newton OSA-based cloud.

To execute:

1. Copy migrate.yml to /opt/openstack-ansible/playbooks

2. Execute the following: openstack-ansible migrate.yml --limit <host>

Caveats:

• It appears that rebooting an instance after the migration has occurred may cause the instance tap to move back to br-int. Working on confiming this behavior
• The bridge check is not quite functional, so the playbook does not fully execute
