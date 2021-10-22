1. Pods
Task:
Using kubectl run command generate pod manifest and save it into /data/task-1.yaml

 Don’t create a pod, just generate its manifest!

Requirements:
Pod name: skodirov-pod
Container image: k8s.gcr.io/pause:3.3
Args: sleep infinity
Labels:
exam=2
task=1


kubectl run skodirov-pod --image=k8s.gcr.io/pause:3.3 --labels="exam=2,task=1" -- command sleep infinity --dry-run=client -o yaml >> /data/task-1.yaml


2. Pods


3. Pods
Create a Pod as per requirements given below:
Pod Namespace: exam-check-02
Pod Name: database-app
Pod Containers:
Init Container:
name: bootstrap
image: busybox
command: sleep 15
Main Container:
name: main
image: redis:3.2-alpine
 Wait till it’s Up and Running

apiVersion: v1
kind: Pod
metadata:
  namespace: exam-check-02
  name: database-app
spec:
  restartPolicy: Never
  initContainers:
  - name: bootsrap
    image: busybox
    command: ["sh", "-c"]
    args:
    - |-
      printenv
      sleep 15
  containers:
  - name: main
    image: redis:3.2-alpine

4. Deployment

Using kubectl create command generate deployment manifest: /data/task-4.yaml

 Don’t create a deployment, just generate its manifest!

Requirements:
Deployment name: skodirov-deploy
Container image: nginx:alpine
Replicas: 3


5. Deployments

Create a deployment as required below:
Name: skodirov-app
Namespace: exam-check-02
Image: nginx:1.17-alpine
Replicas: 3
Container Port: 80
Pods and Deployment Labels:
app: skodirov-app
role: mainapp
tier: backend
student: skodirov
 Ensure that pods are running
