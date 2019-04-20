Deployment service
We use the docker service command to manage Swarm  cluster service, the command can only run the managed node.

New service
Now we are created in a Swarm cluster running a named nginx service.

docker service create --replicas 3 -p 80:80 --name nginx nginx:1.13.7-alpine
w48itofx15qe5k7xin4ystl41
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service converged

View service
Use docker service lsto view the current Swarm service cluster running.

root@ubuntu:# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                 PORTS
w48itofx15qe        nginx               replicated          3/3                 nginx:1.13.7-alpine   *:80->80/tcp

root@ubuntu:# docker service ps nginx
ID                  NAME                IMAGE                 NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
l2a6hl393u3e        nginx.1             nginx:1.13.7-alpine   instance-1          Running             Running about a minute ago
mzfu2id62pl4        nginx.2             nginx:1.13.7-alpine   instance-3          Running             Running about a minute ago
m2yfx73ph8oi        nginx.3             nginx:1.13.7-alpine   instance-2          Running             Running about a minute ago

Use docker service logsto view a log of service.

$ docker service logs nginx

Service scaling
We can use docker service scale the number of containers carried stretch run a service.

When the business is at its peak, we need to expand the number of containers that the service runs.

$ docker service scale nginx=5

When the business is stable, we need to reduce the number of containers that the service runs.

$ docker service scale nginx=2

Delete service

Use docker service rm from the Swarm removal of a service cluster.

$ docker service rm nginx
