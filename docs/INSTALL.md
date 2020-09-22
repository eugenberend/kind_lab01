## Installation

1. Установка `kind`:

```bash
brew install kind
```

2. Запуск кластера:

```bash
kind create cluster --config=cluster/cluster.yaml
kubectl cluster-info --context kind-kind
```

3. Установка `metrics-server`:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml
```

4. Установка `ingress-nginx`:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

5. Ожидание работоспособности `ingress-nginx`:

```bash
kubectl get pods -n ingress-nginx -w
```

6. Установка приложения

```bash
kubectl apply -f app/
```

7. Проверка размещения подов (можно вручную отскейлить до 9-12 и убедиться в равном размещении):

```bash
kubectl get pod -n lab01 -o=custom-columns=NODE:.spec.nodeName,NAME:.metadata.name
```

8. Остановка и удаление кластера:

```bash
kind delete cluster
```