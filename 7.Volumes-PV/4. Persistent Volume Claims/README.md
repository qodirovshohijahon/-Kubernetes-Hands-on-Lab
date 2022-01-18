# 3. Persistent Volume Claims


![](./../img/3-2766dbeb28dfbbdf15ff5280063d98df%20(1).png)

A PersistentVolumeClaim (PVC):
It is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted once read/write or many times read-only).

While PersistentVolumeClaims allow a user to consume abstract storage resources, it is common that users need PersistentVolumes with varying properties, such as performance, for different problems. Cluster administrators need to be able to offer a variety of PersistentVolumes that differ in more ways than just size and access modes, without exposing users to the details of how those volumes are implemented. For these needs, there is the StorageClass resource.

Task:
Please investigate skodirov-data-pv and create persistent volume claim (skodirov-data-pv-claim) for it

Requirements:
PVC Name: skodirov-data-pv-claim
Status: Bound
Documentation:
https://kubernetes.io/docs/concepts/storage/persistent-volumes/
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-requesting-a-raw-block-volume
https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim




1. `skodirov-data-pv.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  finalizers:
  - kubernetes.io/pv-protection
  labels:
    student: skodirov
  name: skodirov-data-pv
  resourceVersion: "1742"
  uid: edf0c939-4203-4510-ba2b-88980ec4f3a8
spec:
  accessModes:
  - ReadOnlyMany
  capacity:
    storage: 200Mi
  local:
    path: /data/skodirov-data-pv
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - node01
  persistentVolumeReclaimPolicy: Retain
  volumeMode: Filesystem
status:
  phase: Available
```

**Sollution**

2. `skodirov-data-pv-claim.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name:  skodirov-data-pv-claim
    namespace: default

    finalizers:
    - kubernetes.io/pvc-protection
spec:
  accessModes:
  - ReadOnlyMany
  selector: 
    matchLabels: 
        student: skodirov
  resources:
    requests:
      storage: 200Mi
  volumeMode: Filesystem
  volumeName: skodirov-data-pv
status:
  accessModes:
  - ReadOnlyMany
  capacity:
    storage: 30Mi
  phase: Bound
```

    student: skodirov
