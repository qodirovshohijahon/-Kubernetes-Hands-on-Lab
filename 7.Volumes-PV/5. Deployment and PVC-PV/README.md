## 4. Deployment and PVC/PV


![](./../img/Screenshot%20from%202022-01-18%2015-09-53.png)

We have created mysql-data pv for you. Please create deployment object and respective persistent volume claim based on following requirements:

Requirements:
Deployment Name: mysql-db
Image Name: mysql:8
Replicas: 1
Environment Variables:
MYSQL_RANDOM_ROOT_PASSWORD: true
MYSQL_DATABASE: production
MYSQL_USER: skodirov
MYSQL_PASSWORD: pAssw0rd
Container Port: 3306
Store MySQL Data in Kubernetes PV:
PV Name: mysql-data
PVC Name: mysql-data-claim
mountPath: /var/lib/mysql
Please find a draft of the application stack in ~/mysql-app.yaml

 Wait till deployment finishes

Note:
For DB and other stateful applications, the best choice is to use StatefulSets!

Verify:
$ kubectl get deployment mysql-db
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
mysql-db   1/1     1            1           10s

$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
mysql-db-5c78556d5c-hsrlb   1/1     Running   0          1m35s

$ kubectl get pvc mysql-data-claim
NAME               STATUS   VOLUME       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-data-claim   Bound    mysql-data   100Mi      ROX                           2m1s

$ kubectl get pv mysql-data
NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                      STORAGECLASS   REASON   AGE
mysql-data   100Mi      ROX            Retain           Bound    default/mysql-data-claim                           2m10s

$ kubectl logs mysql-db-5c78556d5c-hsrlb
...
[MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.18'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
...
Once you finished with this task, please check data folder on local filesystem.

Documentation:
https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/
