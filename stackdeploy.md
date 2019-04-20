Using a compose file in a Swarm cluster

As before using docker-compose.yml to configure once, start multiple containers in Swarm can also use the cluster compose file start multiple services.


swarm deployment using WordPress as an example.

Deployment :

Deployment Services use docker stack deploy, which -c parameter to specify compose the file name.

$ docker stack deploy -c docker-compose.yml wordpress

View service
$ docker stack ls
NAME                SERVICES
wordpress           3

Remove service
To remove the service, use docker stack down

$ docker stack down wordpress
Removing service wordpress_db
Removing service wordpress_visualizer
Removing service wordpress_wordpress
Removing network wordpress_overlay
Removing network wordpress_default
