# lab-elasticsearch-kibana-cerebro
Doing some elasticsearch stuff with 'cerebro' tool and kibana.

This repository contains two docker compose yml files:
* __docker-compose-elastic-node.yml__ : Runs a single node of Elastic Search, an instance of Elastic Search web admin tool called 'Cerebro' and a Kibana container.
* __docker-compose-elastic-cluster.yml__: Runs a Elastic Search cluster with 3 nodes and containers for 'Cerebro' and 'Kibana'.

## Running a single Elastic Search node:

```
docker-compose -f docker-compose-elastic-node.yml up
```
You can retrieve info about your running ES node via:
```
http://localhost:9200/
```

And check its health status with:
```
http://localhost:9200/_cat/health?v
```

The indices created in your ES nodes will be showed in:
```
http://localhost:9200/_cat/indices?v
```

In our case kibana creates a special index called '.kibana' to store info.

### Cerebro web admin tool

We can retrieve info about our ES cluster via ES api and Kibana, but in this lab i want check a web tool called 'Cerebro' to retrieve this info with a friendly and complete user interface.

We can connect to our cluster via 'Cerebro' with next url:
```
http://localhost:9000/#/connect
```
We can see the cluster name, the ES node info, info about '.kibana' index and the unique shard that was created for the index. 
The status of the cluster is reported with the green bar that appears at the top.

![image](https://github.com/joncabrerasu/lab-elasticsearch-kibana-cerebro/blob/master/images/cerebro1.png)

Now we can access to Kibana dashboard and create a test index in ES with a POST. We can execute queries in the 'Dev Tools' Kibana section:
```
POST /lab/default/1 { "title": "First ad in lab ES index" }
```
By default ES creates 5 shards and 1 replica for the new index. In 'Cerebro' we can see the information of the new index, seeing that 5 shards have been created. The status of the cluster is yellow because the replica created by default can not be assigned to any node (remember that we only have one ES node). Replicas must be assigned to a different node by themes of high availability and scalability.


![image](https://github.com/joncabrerasu/lab-elasticsearch-kibana-cerebro/blob/master/images/cerebro-2.png)

## Running a Elastic Search cluster:

It's important to configure the minimum number of nodes in the cluster. It should always be half the number of nodes + 1 to avoid the split brain problem.
```
docker-compose -f docker-compose-elastic-cluster.yml up
```
When we runs this cluster the 3 nodes decides the network topology and negotiates which node will be the master node.

In Cerebro we can see info about the running cluster. The master node is marked with a filled star before its name. We can see 5 shards created for the index (squares printed with a fixed line) an 5 replicas (printed with squares with dotted lines) assigned in balance way to different ES nodes:

![image](https://github.com/joncabrerasu/lab-elasticsearch-kibana-cerebro/blob/master/images/cerebro-cluster-1.png)

With Cerebro we can change the number of replicas. Go to index settings and change the number of replicas to 2. No we can see that the number of replicas increased to 10 (two replicas for each shard):

![image](https://github.com/joncabrerasu/lab-elasticsearch-kibana-cerebro/blob/master/images/cerebro-cluster-replicas.png)

It is also useful to see the usage statistics of each ES node with Cerebro:

![image](https://github.com/joncabrerasu/lab-elasticsearch-kibana-cerebro/blob/master/images/cerebro-cluster-stadistics.png)
