## Try on Docker Swarm Mode using Ansible

Edit `inventories/hosts` to match your cluster

Example:

```
vm1 ansible_host=192.168.56.101
vm2 ansible_host=192.168.56.102

[docker_engine]
vm1
vm2

[docker_swarm_manager]
vm1

[docker_swarm_worker]
vm2
```

Install neccessary Ansible Roles

```bash
ansible-galaxy install -r requirements.yml
```

To form a swarm cluster use `swarm.yml`

```bash
ansible-playbook -i inventories/hosts swarm.yml
```

To start docker services use `services.yml`

```bash
ansible-playbook -i inventories/hosts services.yml
```

Or just run both of them in one playbook use `main.yml`

```bash
ansible-playbook -i inventories/hosts services.yml
```

Once services started try wget one node of the cluster. For this example I will use 192.168.56.101

```bash
watch -n 1 wget -qO- http://192.168.56.101/
```

You should see container id of the processing container each time wget run, this because docker swarm load balance all requests among our cluster.

## Thanks:

- atosatto for [atosatto.docker-swarm](https://github.com/atosatto/ansible-dockerswarm)
- ddrozdov for [docker-compose-swarm-mode)](https://github.com/ddrozdov/docker-compose-swarm-mode)
