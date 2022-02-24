# gitops-manifest

### 準備

サンプルを動かすための環境をkindで構築する
dockerとkubectlコマンドが実行できる必要がある

```
brew install kind
```
mac以外は[公式サイト](https://kind.sigs.k8s.io/)参照

### クラスタ作成

```
# 開発用
kind create cluster --name dev
# 本番用
kind create cluster --name dev

# 確認
kubectl cluster-info context kind-dev
kubectl cluster-info context kind-prod
```

### 削除

```
kind delete cluster --name dev
kind delete cluster --name prod
```

