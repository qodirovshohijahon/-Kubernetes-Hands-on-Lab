### 1. Namespaces
#### Task:
Create Namespace:

Name: exam-check-01
Labels:
created_by=Shokhijakhon_Kodirov
Verify:
$ kubectl get ns --show-labels 
NAME              STATUS   AGE   LABELS
default           Active   88s   <none>
kube-system       Active   88s   <none>
kube-public       Active   88s   <none>
kube-node-lease   Active   88s   <none>
exam-check-01     Active   29s   created_by=Shokhijakhon_Kodirov


Sollution
kind: Namespace
apiVersion: v1
metadata:
  name: exam-check-01
  labels:
    created_by: Shokhijakhon_Kodirov


#### 2. Pods

Check exam-check-01 namespace and answer the questions below:
Q1 How many pods are currently running in exam-check-01 namespace?
Text input
Q2 How many of them are running on node01?
Text input
Q3 From the pods running on node02, what is the highest IP address?
Text input


kubectl get pods -n exam-check-01

 root@client  ~ # kubectl get pods -n exam-check-01 -o wide

#### 3. Pods Creation

Task:
A pod definition file has been placed into /data/ folder.
Please inspect it, resolve all typos and deploy it into exam-check-01 namespace.

 Do the minimal changes.

Verify:
$ kubectl get pods -n exam-check-01 -l created_by="Shokhijakhon_Kodirov"
NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          10s

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: exam-check-01
  labels:
    created_by: Shokhijakhon_Kodirov
spec:
  containers:
  - image: busybox
    name: my-pod
    command: [ "sleep"]
    args: ["infinity"]


#### 4. Inspecting Deployments


Please, inspect pets deployment and answer the questions below:
Q1 What replicas parameter of the deployment is set to?
Text input
Q2 How many ReplicaSets exist in exam-check-01 namespace?
Text input
Q3 Which one (rs) belongs to pets deployment?
Text input
Q4 What image is used for the deployment?
Text input
Q5 What strategy type is chosen?
Text input


Please, inspect pets deployment and answer the questions below:
Q1 What replicas parameter of the deployment is set to?
6
Q2 How many ReplicaSets exist in exam-check-01 namespace?
2
Q3 Which one (rs) belongs to pets deployment?
pets-55dc456887
Q4 What image is used for the deployment?
nginx:alpine
Q5 What strategy type is chosen?
RollingUpdate



6. Inspecting Services

NodePortQuestions:
Q1 What is the type of crowd-service?
NodePort
Q2 What is its Cluster IP?
10.43.152.252
Q3 What is its Service Port?
80
Q4 What is its Target Port?
8080


 root@client  ~ # kubectl get svc --all-namespaces 
NAMESPACE     NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP                  66m
kube-system   kube-dns         ClusterIP   10.43.0.10      <none>        53/UDP,53/TCP,9153/TCP   66m
kube-system   metrics-server   ClusterIP   10.43.72.207    <none>        443/TCP                  66m
ppl           crowd-service    NodePort    10.43.152.252   <none>        80:31363/TCP             77s
 root@client  ~ # kubectl describe service crowd-service -n ppl
Name:                     crowd-service
Namespace:                ppl
Labels:                   app=crowd-app
Annotations:              <none>
Selector:                 app=crowd-app
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.43.152.252
IPs:                      10.43.152.252
Port:                     <unset>  80/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31363/TCP
Endpoints:                10.42.0.10:8080,10.42.0.11:8080,10.42.0.12:8080 + 17 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
 root@client  ~ # 














