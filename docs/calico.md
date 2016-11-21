Calico
===========

Check if the calico-node container is running

```
docker ps | grep calico
```

The **calicoctl** command allows to check the status of the network workloads.
* Check the status of Calico nodes

```
calicoctl node status
```

or for versions prior *v1.0.0*:

```
calicoctl status
```

* Show the configured network subnet for containers

```
 calicoctl get ippool -o wide
```

or for versions prior *v1.0.0*:

```
calicoctl pool show
```

* Show the workloads (ip addresses of containers and their located)

```
calicoctl get workloadEndpoint -o wide
```

and

```
calicoctl get hostEndpoint -o wide
```

or for versions prior *v1.0.0*:

```
calicoctl endpoint show --detail
```

##### Optionnal : BGP Peering with border routers

In some cases you may want to route the pods subnet and so NAT is not needed on the nodes.
For instance if you have a cluster spread on different locations and you want your pods to talk each other no matter where they are located.
The following variables need to be set:
`peer_with_router` to enable the peering with the datacenter's border router (default value: false).
you'll need to edit the inventory and add a and a hostvar `local_as` by node.

```
node1 ansible_ssh_host=95.54.0.12 local_as=xxxxxx
```
