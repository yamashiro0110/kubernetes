# kubernetes

Kubernetesやってみた

## Minikube

localでkubernetesを実行することができる。
> [minikube](minikube/)

## kubectl

install
> https://kubernetes.io/ja/docs/tasks/tools/install-kubectl/
>
> NOTE: [curlでinstallした](https://kubernetes.io/ja/docs/tasks/tools/install-kubectl/#curl%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6macos%E3%81%B8kubectl%E3%81%AE%E3%83%90%E3%82%A4%E3%83%8A%E3%83%AA%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B)

```sh
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:58:59Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:50:46Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}
```

### Helm

Kubernetes Package manager

install
> https://helm.sh/docs/intro/quickstart/

```sh
$ brew install helm
```

```sh
$ helm version
version.BuildInfo{Version:"v3.1.2", GitCommit:"d878d4d45863e42fd5cff6743294a11d28a9abce", GitTreeState:"clean", GoVersion:"go1.13.8"}
```

### monocular

HelmのWebアプリケーション版
> https://github.com/helm/monocular
