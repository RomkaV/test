# Вариант 1:

Запуск minikube
```
minikube start \
    --addons=ingress,dashboard
```

Подключение к внутреннему хранилищу образов minikube и сборка образа:
```
eval $(minikube docker-env)
docker build -t nginx-kefir:1.19.7 -f image/Dockerfile ./image/
```

Запуск helm-чарта:
```
helm install nginx-kefir \
    ./helm/nginx-kefir/ \
    --create-namespace \
    --namespace test1
```

Проверяем, что деплой успешен:
```
kubectl get deployment -n test1
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
nginx-kefir   1/1     1            1           39s
```

Проверка результата:
```
curl -H "Host: example.local" http://$(minikube ip)
```

# Вариант 2:

Запуск helm-чарта:
```
helm install nginx-stock \
    ./helm/nginx-stock/ \
    --create-namespace \
    --namespace test2
```

Проверяем, что деплой успешен:
```
kubectl get deployment -n test2
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
nginx-stock   1/1     1            0           30s
```

Проверка результата:
```
curl -H "Host: example2.local" http://$(minikube ip)
```

Масштабирование:
```
helm install nginx-stock \
    ./helm/nginx-stock/ \
    -n test2 \
    --set replicaCount=10
```

Проверка результата:
```
kubectl get deployment -n test2
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
nginx-stock   10/10   10           10          2m6s
```