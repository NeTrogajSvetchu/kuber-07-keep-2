# kuber-07-keep-2

 kubectl create namespace kub-7
 
 kubectl apply -n kub-7 -f [1dep.yaml](yaml/1dep.yaml)

 kubectl apply -n kub-7 -f [1pvc.yaml ](yaml/1pvc.yaml)

 kubectl apply -n kub-7 -f [1pv.yaml](yaml/1pv.yaml)

 kubectl -n kub-7 get pv

 kubectl -n kub-7 get pvc

 kubectl -n kub-7 get all

 kubectl -n kub-7 get pod # не запускался под статус "Pending"

 kubectl -n kub-7 describe pod dep-1-db88f79f8-dl4x6 
 # ошибка
 ```
 Warning  FailedScheduling  18s   default-scheduler  0/1 nodes are available: 1 node(s) didn't match Pod's node affinity/selector. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
```

```
[kuxar@localhost ~]$  kubectl -n kub-7 describe pod dep-1-5bc4b5f49d-jpswv
Name:             dep-1-5bc4b5f49d-jpswv
Namespace:        kub-7
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=dep-1
                  pod-template-hash=5bc4b5f49d
Annotations:      <none>
Status:           Pending
IP:               
IPs:              <none>
Controlled By:    ReplicaSet/dep-1-5bc4b5f49d
Containers:
  busybox:
    Image:      busybox:1.28
    Port:       <none>
    Host Port:  <none>
    Command:
      sh
      -c
      mkdir -p /scp && while true; do echo "$(date) - Test message" >> /scp/success.txt; sleep 5; done
    Environment:  <none>
    Mounts:
      /scp from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-ff6xb (ro)
  multitool:
    Image:      wbitt/network-multitool
    Port:       <none>
    Host Port:  <none>
    Command:
      sh
      -c
      tail -f /scp/success.txt
    Environment:  <none>
    Mounts:
      /scp from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-ff6xb (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  pvc1
    ReadOnly:   false
  kube-api-access-ff6xb:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              kubernetes.io/hostname=debian
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  58s   default-scheduler  0/1 nodes are available: 1 node(s) didn't match Pod's node affinity/selector. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
```
```
[kuxar@localhost ~]$  kubectl -n kub-7 describe pv pv1
Name:            pv1
Labels:          <none>
Annotations:     pv.kubernetes.io/bound-by-controller: yes
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    local-storage
Status:          Bound
Claim:           kub-7/pvc1
Reclaim Policy:  Delete
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        1Gi
Node Affinity:   <none>
Message:         
Source:
    Type:          HostPath (bare host directory volume)
    Path:          /scp
    HostPathType:  
Events:            <none>
```
```
[kuxar@localhost ~]$  kubectl -n kub-7 describe pvc pvc1
Name:          pvc1
Namespace:     kub-7
StorageClass:  local-storage
Status:        Bound
Volume:        pv1
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Used By:       dep-1-5bc4b5f49d-jpswv
Events:        <none>
```
```
[kuxar@localhost ~]$  kubectl -n kub-7 describe deployment dep-1
Name:                   dep-1
Namespace:              kub-7
CreationTimestamp:      Sat, 14 Sep 2024 11:51:02 +0300
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=dep-1
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=dep-1
  Containers:
   busybox:
    Image:      busybox:1.28
    Port:       <none>
    Host Port:  <none>
    Command:
      sh
      -c
      mkdir -p /scp && while true; do echo "$(date) - Test message" >> /scp/success.txt; sleep 5; done
    Environment:  <none>
    Mounts:
      /scp from volume (rw)
   multitool:
    Image:      wbitt/network-multitool
    Port:       <none>
    Host Port:  <none>
    Command:
      sh
      -c
      tail -f /scp/success.txt
    Environment:  <none>
    Mounts:
      /scp from volume (rw)
  Volumes:
   volume:
    Type:          PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:     pvc1
    ReadOnly:      false
  Node-Selectors:  kubernetes.io/hostname=debian
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   dep-1-5bc4b5f49d (1/1 replicas created)
Events:          <none>
```
Задание 1
Что нужно сделать

Создать Deployment приложения, использующего локальный PV, созданный вручную.

Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
Создать PV и PVC для подключения папки на локальной ноде, которая будет использована в поде.
Продемонстрировать, что multitool может читать файл, в который busybox пишет каждые пять секунд в общей директории.
Удалить Deployment и PVC. Продемонстрировать, что после этого произошло с PV. Пояснить, почему.
Продемонстрировать, что файл сохранился на локальном диске ноды. Удалить PV. Продемонстрировать что произошло с файлом после удаления PV. Пояснить, почему.
Предоставить манифесты, а также скриншоты или вывод необходимых команд.
Задание 2
Что нужно сделать

Создать Deployment приложения, которое может хранить файлы на NFS с динамическим созданием PV.

Включить и настроить NFS-сервер на MicroK8S.
Создать Deployment приложения состоящего из multitool, и подключить к нему PV, созданный автоматически на сервере NFS.
Продемонстрировать возможность чтения и записи файла изнутри пода.
Предоставить манифесты, а также скриншоты или вывод необходимых команд.