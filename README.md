# NoSQL-docker-demo

Governance NoSQL/N1QL Documents with CouchBase

This readme contains materials for understanding NoSQL/N1QL docker..

First step generate a docker-compose.yml file

based of https://blog.couchbase.com/couchbase-using-docker-compose/ 04 2021

and base of this https://github.com/arun-gupta/docker-images/tree/master/couchbase

start a bash or cbq in a terminal

> docker exec -it 8fef74ed1d47 cbq -u Administrator -p password
> cbq> select now_str();

---

Couchbase works inside a swarm setup, too:

> docker swarm init

either with my docker-compose.yml file:

> docker stack deploy -c .\docker-compose.yml c

or via these commands:

> docker service create --name cb-worker --replicas 1 -p 8091:8091 --network couchbase -e TYPE=WORKER -e COUCHBASE=cb-master.couchbase -e AUTO_REBALANCE=false arungupta/couchbase

> docker service create --name cb-worker --replicas 1 --network couchbase -e TYPE=WORKER -e COUCHBASE=cb-master.couchbase -e AUTO_REBALANCE=false arungupta/couchbase

Command to scale

> docker service scale cb-worker=4

Command to maintain and remove

> docker node ls

> docker server list
> ..
> docker server rm <id>

> docker network list
> docker network ..

or

> docker stack rm cb

Commandos for retrieving the IP addresses:

First look for the appropriated CONTAINER ID

> docker ps

For each Couchbase WORKER and MASTER container notifing the internal IP address.
One of these commands return an internal IP address:

> docker inspect --format '{{ .NetworkSettings.IPAddress }}' e64928462521
> or
> docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' b1f90929b927
> 10.0.5.310.0.0.12
