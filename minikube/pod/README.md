# Podをデプロイ

Podをデプロイしてみる。

クラスタ内のエンドポイントにアクセスするために、`busybox-curl`のコンテナをデプロイする。
> https://hub.docker.com/r/yauritux/busybox-curl

## デプロイ

```sh
$ kubectl apply -f sample-pod.yml
```

### デプロイしたPodでコマンドを実行してみる

serviceを確認

```sh
$ kubectl get service

NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
elasticsearch-master            ClusterIP   10.101.30.201   <none>        9200/TCP,9300/TCP   45h
elasticsearch-master-headless   ClusterIP   None            <none>        9200/TCP,9300/TCP   45h
kubernetes                      ClusterIP   10.96.0.1       <none>        443/TCP             45h
```

`elasticsearch-master`の`CLUSTER-IP:PORT`を指定してcurlを実行

```sh
$ kubectl exec sample-pod -- curl 10.101.30.201:9200

{
  "name" : "elasticsearch-master-0",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "8FwFftmJTQmB7LBsjUYgWQ",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
