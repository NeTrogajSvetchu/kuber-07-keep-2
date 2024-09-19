# kuber-07-keep-2

## Задание 1

```
 kubectl create namespace kub-7
```
### Запустил файлики

kubectl apply -n kub-7 -f [1dep.yaml](yaml/1dep.yaml)


 kubectl apply -n kub-7 -f [1pvc.yaml ](yaml/1pvc.yaml)


 kubectl apply -n kub-7 -f [1pv.yaml](yaml/1pv.yaml)

 ### Произвел проверку запуска *.yaml
```
 kubectl -n kub-7 get pv

 kubectl -n kub-7 get pvc

 kubectl -n kub-7 get all

 kubectl -n kub-7 get pod

 kubectl -n kub-7 describe pod dep-1-db88f79f8-dl4x6 
 ```
 ![alt text](png/1.png)

### Проверил что идет запись в файлик "success.txt"
```
 kubectl -n kub-7 exec -it dep-1-75f5887944-wl2ck -c multitool sh 
```

 ![alt text](png/2.png)

kubectl -n kub-7 exec -it dep-2-5fdfd9f8fb-47rnj -c multitool sh 

![alt text]({6FBD7A88-32CA-41F2-B571-4AD6A546A4D5}.png)
### Удалил "pvc" и "dep"
```
kubectl -n kub-7 delete deployment dep-1
kubectl -n kub-7 delete pvc pvc1
kubectl -n kub-7 get pv
kubectl -n kub-7 describe pv pv1
```
При удалении "pvc" и "dep" - "pv" переходит в состояние "Bound(Failed)" так как некчему подключаться данному волуму. 

![alt text](png/4.png)

Файлы которые записывались на данный пв сохранились

![alt text](png/3.png)

Через некоторое время после запуска пвс и деб пв обратно подключился.

Произвожу удаление пв

```
 kubectl -n kub-7 delete pv pv1
```
![alt text](png/5.png)

В моем случае данные которые находились в пв не удалятся так как ом был в режиме
"persistentVolumeReclaimPolicy: Delete"

если бы он был в режиме "persistentVolumeReclaimPolicy: Recycle" тогда данные будут утеряны


### Манифесты :

[1dep.yaml](yaml/1dep.yaml)

[1pvc.yaml ](yaml/1pvc.yaml)

[1pv.yaml](yaml/1pv.yaml)

p/s: Спасибо за помощь с поиском решения ошибки

## Задание 2

### Написал манифесты

kubectl apply -n kub-7 -f [2dep.yaml](yaml/2dep.yaml)

kubectl apply -n kub-7 -f [2pvc.yaml](yaml/2pvc.yaml)
```
kubectl -n kub-7 get pods
```

![alt text](png/6.png)

Ошибка 
```
 Warning  FailedMount  5s (x45 over 16h)  kubelet  MountVolume.SetUp failed for volume "pvc-5eab8fbb-6723-478b-89d6-26aae46048e0" : mount failed: exit status 32
 ```
```
sudo microk8s enable nfs
sudo yum install nfs-utils
sudo mkdir -p /srv/nfs
sudo chown nobody:nogroup /srv/nfs
sudo chmod 0777 /srv/nfs
sudo mv /etc/exports /etc/exports.bak
echo '/srv/nfs 10.0.0.0/24(rw,sync,no_subtree_check)' | sudo tee /etc/exports
sudo systemctl restart nfs*
```
Проверка работоспособности манивестов
```
kubectl -n kub-7 get pods
kubectl -n kub-7 get pvc
kubectl -n kub-7 get pv
```
![alt text](png/7.png)
```
kubectl -n kub-7 exec -it dep-2-5fdfd9f8fb-47rnj -c multitool sh
```
![alt text](png/8.png)
```
kubectl -n kub-7 describe pv pvc-5eab8fbb-6723-478b-89d6-26aae46048e0
```
![alt text](png/10.png)

![alt text](png/9.png)

### Манифесты :

[2dep.yaml](yaml/2dep.yaml)

[2pvc.yaml](yaml/2pvc.yaml)

#########################################



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