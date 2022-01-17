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
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"PersistentVolumeClaim","metadata":{"annotations":{},"creationTimestamp":"2022-01-15T08:35:51Z","finalizers":["kubernetes.io/pvc-protection"],"name":"pv-trouble-claim","namespace":"default","resourceVersion":"5300","uid":"da7ebaf7-47d0-4fcf-be4c-5b36efdfb8bb"},"spec":{"accessModes":["ReadWriteMany"],"resources":{"requests":{"storage":"210Mi"}},"storageClassName":"","volumeMode":"Filesystem"},"status":{"phase":"Pending"}}
  creationTimestamp: "2022-01-15T08:37:04Z"
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
