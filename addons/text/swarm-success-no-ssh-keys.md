To further operate it via docker-machine, add [public SSH keys](https://docs.jelastic.com/ssh-add-key) to your Jelastic account and apply them with the <i>Import Public SSH Keys</i> add-on for the correspoding nodes. 

Alternatively, you can use Jelastic [SSH Gate](https://docs.jelastic.com/ssh-gate) to establish connection to any node within your account.

### Create docker-machine remote connection
```
docker-machine create --driver generic \
--generic-ip-address=${nodes.cp.first.extIPs[0]} \
--generic-ssh-key ~/.ssh/id_rsa \
--engine-storage-driver overlay ${env.envName}
```

### Connect to the environment
```
eval $(docker-machine env ${env.envName})
```

### Add a Manager node to the cluster
```
docker swarm join \
--token ${settings.manager_token} \
${nodes.cp.first.extIPs[0]}:2377
```

### Add a Woker node to the cluster
```
docker swarm join \
--token ${settings.worker_token} \
${nodes.cp.first.extIPs[0]}:2377
```
