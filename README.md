# Ansible MPI Cluster

Ansible project to automatically configure APT-based hosts to act as master/slave nodes in an MPI cluster.

*I should caveat the above by saying that I've only tested the scripts against Ubuntu v14.04. If you discover any problems using the playbook be sure to issue a pull-request.*

## Creating a cluster

To configure your Ubuntu machines as an MPI cluster first create an [inventory file](http://docs.ansible.com/ansible/intro_inventory.html) matching the template:

```yml
master 10.211.55.178

[slaves]
10.211.55.177
10.211.55.175
10.211.55.176
```

Execute the playbooks against the hosts:

```bash
cd ./provisioning
ansible-playbook site.yml -i <PATH TO INVENTORY>
```

### Adding a new slave to the cluster

If you want to add a slave to an existing cluster do the following:

1. Add the slave's IP to the `slaves` group in your inventory file.
2. Run `ansible-playbook slaves.yml -i <PATH TO INVENTORY>`

## Development

[Vagrant](https://www.vagrantup.com/) is used to create development VMs against which you can test changes to the Ansible scripts.

1. [Install Vagrant](https://www.vagrantup.com/docs/installation/).
2. Provision and configure the local cluster with:

```bash
vagrant up
```

Once provisioned the [Ansible Provisioner](https://www.vagrantup.com/docs/provisioning/ansible.html) creates a inventory which can be used manually with the `ansible-playbook` utility:

```bash
cd ./provisioning
ansible-playbook site.yml -i ../.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory
```

Or you can let Vagrant do the provisioning:

```bash
vagrant provision
```
