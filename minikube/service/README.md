# serviceを公開する

minikubeでserviceを公開する。

```sh
$ minikube service SERVICE
```

> @see: `minikube service -h`

## アプリケーションを公開してみる

下記のチュートリアルに沿って、Serviceを公開する。
> https://kubernetes.io/ja/docs/tutorials/hello-minikube/

`Deployment`を作成

```sh
$ kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

`Service`を作成

```sh
$ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

Minikubeで`Service`を公開

```sh
$ minikube service hello-node
```

ブラウザで`Hello World!`が表示される。

***

## 備考

普通のkubernetesでServiceを公開する場合
> https://kubernetes.io/ja/docs/tasks/access-application-cluster/service-access-application-cluster/
