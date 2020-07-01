# Containerized Standalone Spark Cluster

This repo provides containerized Apache Spark, Python 3, and Jupyter notebook environment. The Docker compose file spin up a Spark master node which also runs a jupyter web application, and 3 Spark workers. The number of workers is specified by `replica=3` under `deploy` key as part of the service configuration in the docker-compose.yml file. However, `deploy` key and its options in docker-compose.yml file only work with the the `docker stack deploy` command in Swarm mode and are ignored by `docker compose up`. Therefore, for Docker compose we specify the number of workers with `--scale` flag instead.

First build the "spark" image by running the command bellow:

```bash
docker build -t spark --no-cache Dockerfile-Spark
```

We use three methods to create a standalone Spark cluster; Docker compose, Docker Swarm, and Kubernetes.

## Docker Compose:

Start up the cluster by running:

```bash
docker-compose up --scale sparkworker=3
```

The Jupyter notebook url/token is printed on the terminal, copy and paste the url in your browser to enter Jupyter environment. To veiw Spark master UI, access `localhost:8080` in your browser. Spark application UI can be accessed from `localhost:4040`.

In your terminal press control+c twice to stop Jupyetr server. Then type bellow command to tear down your cluster.

```bash
docker-compose down
```

## Docker Swarm Deployment:

Make sure the the Swarm is enabled on your Docker. To set up Docker Swarm, type `docker swarm init`, and to stop, type `docker swarm leave --force`.

Deploy the cluster to Swarm:

```bash
docker stack deploy --compose-file docker-compose.yml sparkstack
```

Inspect the logs of the spark master container to retrieve the jupyetr notebook url/token.

```bash
docker service logs --raw sparkstack_spark
````

The cluster can be torn down by running:

```bash
docker stack rm sparkstack
```

## Kubernetes Deployment:

We use Docker Desktop's built in Kubernetes environment to deploy our cluster locally. The same docker-compose file is utilized to create a stack on kubernetes.

First make sure the the Kubernetes is enabled on your Docker Desktop. Navigate to Docker preference and make sure there is a green light beside Kubernetes.

```bash
DOCKER_STACK_ORCHESTRATOR=kubernetes docker stack deploy --compose-file docker-compose.yml sparkstack
```

Find out the name of the spark master pod:

```bash
kubectl get pods
```

Inspect the logs of the spark master pod to get the jupyter notebook token.

```bash
kubectl logs {name_of_the_spark_master_pod}
```
