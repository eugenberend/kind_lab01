## Задание

Опишите deployment для веб-приложения в kubernetes в виде yaml-манифеста. Оставляйте в коде комментарии по принятым решениям. Есть следующие вводные:

- [X] у нас мультизональный кластер (три зоны), в котором пять нод
- [X] приложение требует около 5-10 секунд для инициализации
- [X] по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
- [X] на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU
- [X] По памяти всегда “ровно” в районе 128M memory
- [X] приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
- [X] хотим максимально отказоустойчивый deployment
- [X] хотим минимального потребления ресурсов от этого deployment’а


## kind

Install:

```bash
brew install kind
```

Start cluster:

```bash
kind create cluster --config=cluster/cluster.yaml
kubectl cluster-info --context kind-kind
```

## HPA

Install:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml
```

## Ingress

Install:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

Check nginx pods:

```bash
kubectl get pods -n ingress-nginx -w
```

## Application

Apply:

```bash
kubectl apply -f app/
```

Check pods placement:

```bash
kubectl get pod -n lab01 -o=custom-columns=NODE:.spec.nodeName,NAME:.metadata.name
```

Stop cluster:

```bash
kind delete cluster
```
