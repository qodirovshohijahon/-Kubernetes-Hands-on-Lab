## 2. PV / PVC Troubleshooting

![](../img/2-persist.png)

![](../img/Screenshot%20from%202022-01-15%2012-48-41.png)

We have created pv and pvc for you. But something went wrong and pvc didn’t bind to pv. Please troubleshoot and fix pvc

$ kubectl get pvc pv-trouble-claim
NAME               STATUS    ...
pv-trouble-claim   Pending   
Requirements:
PV Name: pv-trouble
PVC Name: pv-trouble-claim
 Don’t change PV

Should be like this:
$ kubectl get pvc
NAME               STATUS   VOLUME      ...
pv-trouble-claim   Bound    pv-trouble  ...

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  finalizers:
  - kubernetes.io/pv-protection
  name: skodirov-data-pv
spec:
  accessModes:
  - ReadOnlyMany
  capacity:
    storage: 30Mi
  local:
    path: /data
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
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  finalizers:
  - kubernetes.io/pvc-protection
  name: pv-trouble-claim
  namespace: default
  resourceVersion: "5373"
  uid: fdc4ba57-862f-4a84-915e-1c28b18126e1
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 210Mi
  storageClassName: ""
  volumeMode: Filesystem
status:
  phase: Pending
```

Documentation:
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modess

1. `pv-trouble.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  finalizers:
  - kubernetes.io/pv-protection
  name: pv-trouble
  resourceVersion: "6847"
  uid: 178c4c76-8177-4632-8b0f-712a7b8a944d
spec:
  accessModes:
  - ReadOnlyMany
  capacity:
    storage: 30Mi
  local:
    path: /data/pv-trouble
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

2. `pv-trouble-claim.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  finalizers:
  - kubernetes.io/pvc-protection
  name: pv-trouble-claim
  namespace: default
  resourceVersion: "6848"
  uid: ecb9d1a4-2af7-4b2b-8c73-9ffbd8a5279c
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 210Mi
  volumeMode: Filesystem
  volumeName: pv-trouble
```

### bind pv-trouble-claim to pv-trouble

***Sollution**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  finalizers:
  - kubernetes.io/pvc-protection
  name: pv-trouble-claim
  namespace: default
  resourceVersion: "1526"
  uid: dd4eccdc-cfc0-4d44-bbbb-da72dcd1d63e
spec:
  accessModes:
  - ReadOnlyMany
  resources:
    requests:
      storage: 30Mi
  volumeMode: Filesystem
  volumeName: pv-trouble
status:
  accessModes:
  - ReadOnlyMany
  capacity:
    storage: 30Mi
  phase: Bound
```

![](./../img/Screenshot%20from%202022-01-18%2014-55-36.png)

**Links**

- https://intl.cloud.tencent.com/document/product/457/37770
- https://stackoverflow.com/questions/34282704/can-a-pvc-be-bound-to-a-specific-pv
- https://loft.sh/blog/kubernetes-persistent-volumes-examples-and-best-practices/
- 