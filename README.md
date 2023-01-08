# Домашнее задание к занятию "13.1 контейнеры, поды, deployment, statefulset, services, endpoints"

## Задание 1: подготовить тестовый конфиг для запуска приложения
Для начала следует подготовить запуск приложения в stage окружении с простыми настройками. Требования:
* под содержит в себе 2 контейнера — фронтенд, бекенд;
* регулируется с помощью deployment фронтенд и бекенд;
* база данных — через statefulset.

### Ответ:
* cобрал докер образы фронта и бэка и залил на docker hub
* создал namespase для stage
* ссылка на [deployment.yaml](stage/deployment.yaml)
* ссылка на [db.yaml](stage/db.yaml)


```
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get po
NAME                      READY   STATUS    RESTARTS   AGE
db-0                      1/1     Running   0          5h23m
fb-app-77d9c5cb44-x7pmn   2/2     Running   0          5h25m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get deployments
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
fb-app   1/1     1            1           5h25m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get statefulset
NAME   READY   AGE
db     1/1     5h23m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get services
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
backend    ClusterIP   10.233.29.234   <none>        9000/TCP   5h25m
db         ClusterIP   10.233.30.191   <none>        5432/TCP   5h23m
frontend   ClusterIP   10.233.39.149   <none>        80/TCP   5h25m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$
```

## Задание 2: подготовить конфиг для production окружения
Следующим шагом будет запуск приложения в production окружении. Требования сложнее:
* каждый компонент (база, бекенд, фронтенд) запускаются в своем поде, регулируются отдельными deployment’ами;
* для связи используются service (у каждого компонента свой);
* в окружении фронта прописан адрес сервиса бекенда;
* в окружении бекенда прописан адрес сервиса базы данных.

### Ответ:
* те же образы фронта и бэка из docker hub
* создал namespase для prod
* ссылка на [frontend.yaml](prod/frontend.yaml)
* ссылка на [backend.yaml](prod/backend.yaml)
* ссылка на [db.yaml](prod/db.yaml)

```
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n prod get po
NAME                        READY   STATUS    RESTARTS   AGE
backend-64969cddb6-9cnw4    1/1     Running   0          2m20s
db-0                        1/1     Running   0          2m
frontend-654946877b-v69ns   1/1     Running   0          2m7s
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n prod get deployments
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
backend    1/1     1            1           2m32s
frontend   1/1     1            1           2m19s
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n prod get statefulset
NAME   READY   AGE
db     1/1     2m23s
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n prod get services
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
backend    ClusterIP   10.233.7.148    <none>        9000/TCP   2m53s
db         ClusterIP   10.233.36.239   <none>        5432/TCP   2m33s
frontend   ClusterIP   10.233.51.191   <none>        80/TCP     2m40s
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n prod exec frontend-654946877b-v69ns -- curl localhost
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   448  100   448    0     0  49777      0 --:--:-- --:--:-- --:--:-- 49777
<!DOCTYPE html>
<html lang="ru">
<head>
    <title>Список</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="/build/main.css" rel="stylesheet">
</head>
<body>
    <main class="b-page">
        <h1 class="b-page__title">Список</h1>
        <div class="b-page__content b-items js-list"></div>
    </main>
    <script src="/build/main.js"></script>
</body>
</html>vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$
```
