# docker-swarm-without-global-mode
we have three node cluster in docker and we want to replicated or scale up from 1 to 3 but we want each container should be run on each node and we don't want global mode of the service.

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
  
  
