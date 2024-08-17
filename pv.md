```bash
lsblk
 1001  fdisk /dev/vdb 
 1002  lsblk
 1003  mkfs.ext4 /dev/vdb1 
 1004  lsblk
 1005  vi /etc/fstab 
 1006  mount -a
 1007  mkdir /local-pv
 1008  mount -a
 1009  lsblk

```
```bash
UUID=38b1e987-a6fd-4593-bd74-325874b3cf72     /           xfs    defaults,noatime  1   1
UUID=11EB-AD1C        /boot/efi       vfat    defaults,noatime,uid=0,gid=0,umask=0077,shortname=winnt,x-systemd.automount 0 2
/dev/vdb1	/local-pv ext4	defaults,noatime	1 1
```
* pv.yml
```bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: k8challenge-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /local-pv 
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - k8-challenge
```
* Storage class sc.yml
```bash
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: k8challenge-sc
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```
* create pvc
```bash
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: k8challenge-sc
  resources:
    requests:
      storage: 1Gi

```
* use pvc in a pod 
```bash
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-pvc
spec:
  containers:
    - name: my-container
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: local-storage
  volumes:
    - name: local-storage
      persistentVolumeClaim:
        claimName: my-pvc
```
```bash
In Kubernetes, the volumeBindingMode field in the StorageClass specification determines when the volume binding and dynamic provisioning should occur. The supported values for volumeBindingMode are:

Immediate:

Volume binding and provisioning occur immediately once the PersistentVolumeClaim (PVC) is created.
This is the default mode if volumeBindingMode is not specified.
Suitable for storage types that are not topology-constrained (e.g., cloud-based storage that can be provisioned without considering the node's location).
WaitForFirstConsumer:

Volume binding and provisioning are delayed until a Pod using the PVC is scheduled.
This mode allows the scheduler to consider the Pod's scheduling constraints (like node affinity, zone, etc.) before provisioning the volume.
Suitable for topology-constrained storage (e.g., local storage, regional storage that must be provisioned in a specific zone).
Here is an example of a StorageClass with the volumeBindingMode specified:

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: example-storageclass
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer  # or Immediate
Examples of Each Mode
Immediate Binding Mode

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
volumeBindingMode: Immediate

In this example, an AWS EBS volume of type gp2 will be provisioned immediately when a PVC is created.

WaitForFirstConsumer Binding Mode

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer

In this example, the local storage volume will be provisioned and bound only when a Pod that uses the PVC is scheduled, allowing the scheduler to place the Pod based on the node's location and other constraints.

Choosing the appropriate volumeBindingMode depends on your specific use case and the type of storage you are working with.
```