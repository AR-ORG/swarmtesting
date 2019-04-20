SWarm mode and rolling upgrade
In deployement section ,  we use nginx:1.13.7-alpine deployed a named nginx service.

Now we want to NGINX upgrade to a version 1.13.12, how to upgrade Swarm mode in services?

You might think of stopping the original service, deploying a service with a new image, and not completing the "upgrade" of the service.

The drawbacks of this are obvious. If there is a problem with the newly deployed service, it is difficult to recover after the original service is deleted. So how do you perform a rolling upgrade of the service in Swarm mode?

The answer is to use the docker service update command.

$ docker service update \
    --image nginx:1.13.12-alpine \
    nginx
The above command --imageoption to update the service's image. Of course, we can also be used docker service update to update any configuration.


More Options can docker service update -h view the command.

Service rollback
Now suppose we found nginxthe upgrade to the image of the service nginx:1.13.12-alpinethere are some problems, we can use a key command rolled back.

$ docker service rollback nginx

Now use the docker service ps command to view nginx service details.

root@ubuntu:# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                  PORTS
w48itofx15qe        nginx               replicated          3/3                 nginx:1.13.12-alpine   *:80->80/tcp
root@ubuntu:# docker service update  --image nginx:latest    nginx
nginx
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service converged
root@ubuntu:/home/sasmeetaroutray# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
w48itofx15qe        nginx               replicated          3/3                 nginx:latest        *:80->80/tcp
root@ubuntu:# docker service rollback nginx
nginx
rollback: manually requested rollback
overall progress: rolling back update: 3 out of 3 tasks
1/3: running   [>                                                  ]
2/3: running   [>                                                  ]
3/3: running   [>                                                  ]
verify: Service converged
root@ubuntu:# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                  PORTS
w48itofx15qe        nginx               replicated          3/3                 nginx:1.13.12-alpine   *:80->80/tcp

root@ubuntu:# docker service ps nginx
ID                  NAME                IMAGE                  NODE                DESIRED STATE       CURRENT STATE                 ERROR               PORTS
uje9y6gwf3l0        nginx.1             nginx:1.13.12-alpine   instance-2          Running             Running about a minute ago
l4979sqz80hb         \_ nginx.1         nginx:latest           instance-1          Shutdown            Shutdown about a minute ago
6s4eaujcruec         \_ nginx.1         nginx:1.13.12-alpine   instance-1          Shutdown            Shutdown about a minute ago
slxfvxqkqgon         \_ nginx.1         nginx:latest           instance-3          Shutdown            Shutdown 2 minutes ago
tdax4k1piend         \_ nginx.1         nginx:1.13.12-alpine   ubuntu              Shutdown            Shutdown 3 minutes ago
0ihxbedzbl9r        nginx.2             nginx:1.13.12-alpine   instance-1          Running             Running about a minute ago
t26hlgo077r5         \_ nginx.2         nginx:latest           instance-3          Shutdown            Shutdown about a minute ago
w4r85rj5mrx7         \_ nginx.2         nginx:1.13.12-alpine   ubuntu              Shutdown            Shutdown about a minute ago
ir0q6y8x05oh         \_ nginx.2         nginx:latest           ubuntu              Shutdown            Shutdown 2 minutes ago
jkfsopr3tv4a         \_ nginx.2         nginx:1.13.12-alpine   instance-2          Shutdown            Shutdown 3 minutes ago
7qsp8s1oniwv        nginx.3             nginx:1.13.12-alpine   ubuntu              Running             Running about a minute ago
im07if4b2tb5         \_ nginx.3         nginx:latest           ubuntu              Shutdown            Shutdown about a minute ago
026fc3tqc6t1         \_ nginx.3         nginx:1.13.12-alpine   instance-2          Shutdown            Shutdown about a minute ago
osmzfknce0o4         \_ nginx.3         nginx:latest           instance-1          Shutdown            Shutdown 2 minutes ago
m7y23euvyh5n         \_ nginx.3         nginx:1.13.12-alpine   instance-1          Shutdown            Shutdown 3 minutes ago
The output of the results details the process of service deployment, rolling upgrade, and rollback.
