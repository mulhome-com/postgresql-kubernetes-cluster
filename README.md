# postgresql-kubernetes-cluster
Build the postgresql cluster in the kubernetes

Prerequisites:

- Linux
- Kubernetes
- NFS
- Sysbench


Process:

> kubectl -n ${namespace} apply -f postgres-configmap.yaml

- Generate the some begining scripts in the configmap, just like master-slave-config.sh, create-replication-role.sql and alter-postgres-password.sql


> kubectl -n ${namespace} apply -f postgres-service.yaml

- Generate the service for statefulset


> kubectl -n ${namespace} apply -f postgres-statefulset.yaml

- Generate the cluster by statefulset


> kubectl -n ${namespace} apply -f postgres-service-master.yaml

- Create on service for reading and writing



Test:


> kubectl -n ${namespace} get svc

- List the service for node port


read-write-service: 31386

read-only-service: 30685 

pod-server: testsvr



- Run the sysbench test

> sysbench /usr/share/sysbench/oltp_read_only.lua --threads=48 --tables=50 --db-driver=pgsql --pgsql-host=testsvr --pgsql-user=postgres --pgsql-password=IKge6geoZHio --pgsql-port=31386 --pgsql-db=postgres prepare 

> sysbench /usr/share/sysbench/oltp_read_only.lua --threads=48 --tables=50 --db-driver=pgsql --pgsql-host=testsvr --pgsql-user=postgres --pgsql-password=IKge6geoZHio --pgsql-port=30685 --pgsql-db=postgres run


Get the result

>    queries:                             176304 (17262.41 per sec.)

