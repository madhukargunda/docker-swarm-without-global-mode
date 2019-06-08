# docker-swarm-without-global-mode
```
we have three node cluster in docker and we want to replicated or scale up from 1 to 3 but we want each container should be run on each node and we don't want global mode of the service.

```
## Placement Prefrences

- 1.We can set up the service to divide tasks evenly over different categories of nodes.

- 2.This uses --placement-pref with a spread strategy (currently the only supported strategy) to spread tasks evenly over 
    the values of the datacenter node label.
    
- 3.Lets take example each node has the datacenter label attached to it.
    
    Three nodes with node.labels.datacenter=east
    Two nodes with node.labels.datacenter=south
    One node with node.labels.datacenter=west
    
- 4.Since we are spreading over the values of the datacenter label and the service has 9 replicas, 3 replicas will end up in 
    each datacenter. There are three nodes associated with the value east, so each one will get one of the three replicas 
    reserved for this value. There are two nodes with the value south, and the three replicas for this value will be divided 
    between them, with one receiving two replicas and another receiving just one. Finally, west has a single node that will 
    get all three replicas reserved for west.
 
- 5.docker service create \
     --replicas 9 \
      --name redis_2 \
      --placement-pref 'spread=node.labels.datacenter' \
    redis:3.0.6

- 6.If the nodes in one category (for example, those with node.labels.datacenter=south) 
    can’t handle their fair share of tasks due to constraints or resource limitations, 
    the extra tasks will be assigned to other nodes instead, if possible.
    
- 7. Both engine labels and node labels are supported by placement preferences

- 8.

 ocker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref 'spread=node.labels.datacenter' \
  --placement-pref 'spread=node.labels.rack' \
  redis:3.0.6

- 9. 



```
   docker-machine create --driver virtualbox manager1
   docker-machine create --driver virtualbox worker1
   docker-machine create --driver virtualbox worker2
   docker-machine create --driver virtualbox worker3
   docker-machine create --driver virtualbox worker4

   eval $(docker-machine env manager1)
   docker swarm init --advertise-addr $(docker-machine ip manager1)
   eval $(docker-machine env worker1)
   docker swarm join --token SWMTKN-1-03oxz047h9pd0dj4bu3yw7hkxr9e8e9m1ug17fg2f6xyu3m4hy-cwzjkr1ecfxzqw5gyrn5bl6ie 192.168.99.100:2377
   eval $(docker-machine env worker2)
   docker swarm join --token SWMTKN-1-03oxz047h9pd0dj4bu3yw7hkxr9e8e9m1ug17fg2f6xyu3m4hy-cwzjkr1ecfxzqw5gyrn5bl6ie 192.168.99.100:2377
   eval $(docker-machine env worker3)
   docker swarm join --token SWMTKN-1-03oxz047h9pd0dj4bu3yw7hkxr9e8e9m1ug17fg2f6xyu3m4hy-cwzjkr1ecfxzqw5gyrn5bl6ie 192.168.99.100:2377
   eval $(docker-machine env manager1)

  docker node update --label-add my.pref=1 worker1
  docker node update --label-add my.pref=2 worker2
  docker node update --label-add my.pref=3 worker3
  docker node update --label-add my.pref=4 worker4  
  
  ``
