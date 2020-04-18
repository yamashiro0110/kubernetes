# Elasticsearchを動かしてみる

minikubeでElasticsearchを動かしてみる。

## Elasticsearchをデプロイ

Helmでinstallする。
> https://hub.helm.sh/charts/stable/elasticsearch/1.32.4

```sh
$ helm install stable/elasticsearch --version 1.32.4
```

## Serviceを作成

Elasticsearchに外部からアクセスできるようにするため、NodePortを作成する。
> [nodeport.yml](nodeport.yml)

```sh
$ kubectl apply -f nodeport.yml
```

## Serviceを公開

作成したServiceを確認

```sh
$ kubectl get service
```

minikubeでserviceを公開する。

```sh
$ minikube service es-nodeport
```
