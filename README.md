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
db-0                      1/1     Running   0          17m
fb-app-77d9c5cb44-x7pmn   2/2     Running   0          19m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get deployments
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
fb-app   1/1     1            1           20m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get statefulset
NAME   READY   AGE
db     1/1     18m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$ kubectl -n stage get services
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
backend    ClusterIP   10.233.29.234   <none>        9000/TCP   20m
db         ClusterIP   10.233.30.191   <none>        5432/TCP   18m
frontend   ClusterIP   10.233.39.149   <none>        8000/TCP   20m
vagrant@vagrant:~/netology-13-kubernetes-config-01-objects$
```
