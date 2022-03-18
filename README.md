# gitops-manifest

### 準備

確認時のバージョン

```
$ kubectl version --short
Client Version: v1.22.5
Server Version: v1.21.1

# helm はbrewでinstallしたもの
$ helm version --short
v3.8.0+gd141386
```

サンプルを動かすための環境をkindで構築
実行にはdockerとkubectlコマンドが実行できる必要がある

```
brew install kind
```
mac以外は[公式サイト](https://kind.sigs.k8s.io/)参照

### クラスタ作成

```
# 開発用
kind create cluster --name dev
# 本番用
kind create cluster --name prod

# 確認
kind get clusters

kubectl cluster-info context kind-dev
kubectl cluster-info context kind-prod
```

### 削除

```
kind delete -A

# 個別削除する場合
kind delete cluster --name dev
kind delete cluster --name prod
```

### ArgoCD

```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 初期PW
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

kubectl -n argocd port-forward svc/argocd-server 8000:80
```

### Prometheus

helmを使用して構築
```
helm ls -A
# リポジトリ追加
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helmerepo list

# チャート検索
helm search repo prometheus-community

# namespace作成
kubectl create ns monitoring

# チャートインストール
helm install prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring

# 確認
kubectl get pod -n monitoring -w
kubectl --namespace monitoring get pods -l "release=prometheus-stack"
helm ls -n monitoring

kubectl get node -o wide

kubectl get svc -n monitoring

# kindで構築した場合、portNode利用するには起動時に設定が必要
# port-forwardで動作確認
kubectl port-forward svc/prometheus-stack-kube-prom-prometheus 9090
curl localhost:9090

```



