Create a Swarm cluster

Initialize the cluster

We first create a swarm cluster which will be having multiple nodes. So in this demo , i have created 3 node manager node and 1 worker node

login to the one node, which you want to be as a manager node.

~$ docker swarm init
Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.


Increase work node

worker1:~$ docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

This node joined a swarm as a worker.

View cluster
After the top two steps, we already have a minimum Swarm cluster includes 3 management nodes and 1 worker node.

Use the management node docker node ls view the cluster.

root@ubuntu:/home/root# docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS    ENGINE VERSION
wc9iinw70yjp1ita06ky5vm8k     instance-1          Ready               Active              Leader              18.09.5
ryf8t7qdng5ai8lu8adnfew3f     instance-2          Ready               Active                                  18.09.5
zakh7yyah9txz7y1ofp9n49yk     instance-3          Ready               Active              Reachable           18.09.5
3a1gn5ink2sd11mu3zh8xzw4y *   ubuntu              Ready               Active              Reachable           18.09.5
