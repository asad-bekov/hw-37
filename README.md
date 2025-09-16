# Домашнее задание: «Сетевое взаимодействие в Kubernetes»

> Репозиторий: hw-37\
> Выполнил: Асадбек Асадбеков\
> Дата: сентябрь 2025\
> Ссылка: [Манифесты](https://github.com/asad-bekov/hw-37/blob/main/manifests/)


## Цель задания

Научиться настраивать доступ к приложениям в Kubernetes:
- Внутри кластера через **Service** (`ClusterIP`, `NodePort`).
- Снаружи кластера через **Ingress**.

---

## Задание 1: Настройка Service (ClusterIP и NodePort)

### Задача

Развернуть приложение из двух контейнеров (nginx и multitool) и обеспечить доступ к ним:
- Внутри кластера через ClusterIP.
- Снаружи через NodePort.

### Манифесты

- [`deployment-multi-container.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/deployment-multi-container.yaml) — Deployment с двумя контейнерами (**nginx** и **multitool**).
- [`service-clusterip.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/service-clusterip.yaml) — Service типа ClusterIP.
- [`service-nodeport.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/service-nodeport.yaml) — Service типа NodePort для доступа к **nginx** снаружи.

---

### Результаты проверки

#### 1. Доступ внутри кластера (ClusterIP)

```bash
# Из временного пода
kubectl run test-pod --image=wbitt/network-multitool --rm -it --restart=Never -- sh
curl multi-container-service:9001 # nginx
curl multi-container-service:9002 # multitool
```
Оба запроса выполнены успешно.

#### 2. Доступ снаружи кластера (NodePort)

```bash
curl http://localhost:30080 # nginx
```
Запрос выполнен успешно.

---

### Скриншоты

- ![Очистка окружения (kubectl get all)](https://github.com/asad-bekov/hw-37/blob/main/img/1.PNG)
- ![Deployment и Pods](https://github.com/asad-bekov/hw-37/blob/main/img/2.PNG)
- ![Service ClusterIP](https://github.com/asad-bekov/hw-37/blob/main/img/3.PNG)
- ![Проверка ClusterIP](https://github.com/asad-bekov/hw-37/blob/main/img/4.PNG)
- ![Service NodePort](https://github.com/asad-bekov/hw-37/blob/main/img/5.PNG)
- ![Проверка NodePort](https://github.com/asad-bekov/hw-37/blob/main/img/6.PNG)

---

## Задание 2: Настройка Ingress

### Задача

Развернуть два приложения (`frontend` и `backend`) и обеспечить доступ к ним через **Ingress** по разным путям.

### Манифесты

- [`deployment-frontend.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/deployment-frontend.yaml) — Deployment для frontend (**nginx**).
- [`deployment-backend.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/deployment-backend.yaml) — Deployment для backend (**multitool**).
- [`service-frontend.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/service-frontend.yaml) — Service для frontend.
- [`service-backend.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/service-backend.yaml) — Service для backend.
- [`ingress.yaml`](https://github.com/asad-bekov/hw-37/blob/main/manifests/ingress.yaml) — Ingress для маршрутизации по путям.

---

### Результаты проверки
```bash
curl http://localhost/     # -> frontend (nginx)
curl http://localhost/api  # -> backend (multitool)
```
Оба запроса выполнены успешно.  
**Ingress корректно маршрутизирует трафик по путям.**

---

### Скриншоты
- ![Deployments и Services](https://github.com/asad-bekov/hw-37/blob/main/img/7.PNG)
- ![Ingress Controller Pod](https://github.com/asad-bekov/hw-37/blob/main/img/8.PNG)
- ![Ingress Resource](https://github.com/asad-bekov/hw-37/blob/main/img/9.PNG)
- ![Проверка Ingress](https://github.com/asad-bekov/hw-37/blob/main/img/10.PNG)

---

## Как запустить

### Применить все манифесты:

```bash
kubectl apply -f deployment-multi-container.yaml
kubectl apply -f service-clusterip.yaml
kubectl apply -f service-nodeport.yaml
kubectl apply -f deployment-frontend.yaml
kubectl apply -f deployment-backend.yaml
kubectl apply -f service-frontend.yaml
kubectl apply -f service-backend.yaml
kubectl apply -f ingress.yaml
```

### Включить Ingress (для MicroK8s):

```bash
microk8s enable ingress
```

---

Теперь приложения доступны как внутри кластера, так и снаружи через **Service** и **Ingress**.
