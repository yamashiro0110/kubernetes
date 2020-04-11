# Dynamic Volume Provisioning

MinikubeでDynamic Volume Provisioningを使ってみる。

Dynamic Volume Provisioningについて
> https://kubernetes.io/ja/docs/concepts/storage/dynamic-provisioning/

## デフォルトのStorageClass

minikubeをインストールすると、デフォルトで`standard`というStorageClassが存在している。

```sh
$ kubectl get sc standard -o wide
NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  7m38s
```

`PersistentVolumeClaim`を作成するときに、`StorageClass`を指定しない場合はデフォルトの`StorageClass`が利用される。
> https://kubernetes.io/ja/docs/concepts/storage/dynamic-provisioning/#%E3%83%87%E3%83%95%E3%82%A9%E3%83%AB%E3%83%88%E3%81%AE%E6%8C%99%E5%8B%95

`StorageClass`(`standard`)のアノテーション(`storageclass.kubernetes.io/is-default-class`)を確認すると`true`になっている。

```sh
$ kubectl get sc standard -o jsonpath="{.metadata.annotations['storageclass\.kubernetes\.io/is-default-class']}"

true
```

## PersistentVolumeClaimを作成

`StorageClass`を指定せずに`PersistentVolumeClaim`を作成する。
> [dynamic-provisioning-test-pvc.yml](dynamic-provisioning-test-pvc.yml)

```sh
$ kubectl apply -f dynamic-provisioning-test-pvc.yml
```

`PersistentVolumeClaim`が作成されているか確認。

```sh
$ kubectl get pvc
NAME                            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
dynamic-provisioning-test-pvc   Bound    pvc-1b403fb0-3d75-49e9-8bd1-73d65a1b6cbe   3Gi        RWO            standard       4m28s
```

## PersistentVolumeClaimを指定してPodを作成

`PersistentVolumeClaim`(`dynamic-provisioning-test-pvc`)を指定して`Pod`を作成する。
> [dynamic-provisioning-test-pod.yml](dynamic-provisioning-test-pod.yml)

```sh
$ kubectl apply -f dynamic-provisioning-test-pod.yml
```

`Pod`が作成されているか確認。

```sh
$ kubectl get pod
NAME                            READY   STATUS    RESTARTS   AGE
dynamic-provisioning-test-pod   1/1     Running   0          6m46s
```

ディレクトリがmountされているか確認。
> `/data/test`ディレクトリが存在している

```sh
$ kubectl exec dynamic-provisioning-test-pod -- ls -la /data/
total 12
drwxr-xr-x    3 root     root          4096 Apr 11 12:50 .
drwxr-xr-x    1 root     root          4096 Apr 11 12:50 ..
drwxrwxrwx    2 root     root          4096 Apr 11 12:35 test
```

## PersistentVolumeClaimを指定してDeploymentを作成

`PersistentVolumeClaim`(`dynamic-provisioning-test-pvc`)を指定して`Deployment`を作成する。
> [dynamic-provisioning-test-deployment.yml](dynamic-provisioning-test-deployment.yml)

```sh
$ kubectl apply -f dynamic-provisioning-test-deployment.yml
```

`Deployment`が作成されているか確認。

```sh
$ kubectl get deployments
NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
dynamic-provisioning-test-deployment   3/3     3            3           12m
```

`Deployment`から`Pod`が作成されている。

```sh
$ kubectl get pods -l app=dynamic-provisioning-test-deployment
NAME                                                    READY   STATUS    RESTARTS   AGE
dynamic-provisioning-test-deployment-675c87448b-6zcl2   1/1     Running   0          96s
dynamic-provisioning-test-deployment-675c87448b-k8kvh   1/1     Running   0          90s
dynamic-provisioning-test-deployment-675c87448b-pn8vd   1/1     Running   0          102s
```

ディレクトリがmountされているか確認。
> 残りの2つもmountされていた

```sh
$ kubectl exec dynamic-provisioning-test-deployment-675c87448b-6zcl2 -- ls -l /data/test/
total 4
drwxrwxrwx    2 root     root          4096 Apr 11 12:35 deloyment
```

***

## 備考

- https://cstoku.dev/posts/2018/k8sdojo-12/
- https://kubernetes.io/ja/docs/concepts/workloads/pods/pod/
- https://kubernetes.io/ja/docs/concepts/storage/dynamic-provisioning/
- https://kubernetes.io/ja/docs/concepts/workloads/controllers/deployment/
