# Пошаговуая инструкция для разворачивания сервиса регистрации зюзюбликов в кластере Kubernetes.  

## Состав репозитория:  

```bash
├── app
│   ├── 00-namespace.yaml            - Создание пространства имен
│   ├── 01-postgres-secret.yaml      - Создание secret для postgres
│   ├── 02-postgres-pvc.yaml         - Конфигурация постоянного тома
│   ├── 03-postgres-statefulset.yaml - Разворачивание базы данных PostgreSQL
│   ├── 04-postgres-service.yaml     - Настройка доступа к PostgreSQL
│   ├── 05-app-configmap.yaml        - ConfigMap для разворачиваемого приложения
│   ├── 06-app-deployment.yaml       - Deployment для приложения
│   ├── 07-app-service.yaml          - Создание службы для организации внешнего доступа к веб-приложению
│   └── 08-ingress.yaml              - Настройка маршрутизации внешнего доступа к приложению
└── Readme.md                        - Этот файл

```  
### Предполагается, что в кластере установлен ingress контроллер.  
   
## 1 шаг. Нужно скачать данный редозиторий.  

Введите в терминале команду:  
``` bash
git clone https://github.com/Yury-Piataev/zuozuo.git
```  

## 2 шаг. Запуск разворачивания сервиса регистрации.
   
Введите в терминале команду:  
``` bash
kubectl apply -f app/
```  
   
## 3 шаг. Проверка статуса готовгности сервиса.    
  
Введите в терминале команду:  
``` bash  
kubectl get pods -n zouzou-ns
```  
Если вывод выглядит подобным образом:  
```bash
NAME                                READY   STATUS             RESTARTS      AGE
postgres-0                          1/1     Running            0             85s
zouzoublique-api-5fc7884d5d-8b8zb   1/1     Running            0             85s
```  
то сервис установлен.  

## 4 шаг. Получение внешнего ip для запроса к сервису регистрации.

Введите в терминале команду:  
``` bash  
kubectl get ingress -n zouzou-ns
```  
Получите вывод типа: 
```dash
NAME                   CLASS   HOSTS      ADDRESS     PORTS   AGE
zouzoublique-ingress   nginx   hello.ex   10.0.2.15   80      13m
``` 
В поле ADDRESS будет указан ваш ip адрес.

## 5 шаг. Регистрация зюзюблика.  

Для регистрации зюзюблика введите в терминале команду:

```dash
curl -X POST   http://ваш_ip/v1/zouzoubliques -H "Content-Type: application/json" -d '{"name": "зюзюблик 1", "type": 1}'
```  

## 6 шаг. Проверка добавленного зюзюблика. 

Для проверки зарегистрированного зюзюблика введите в терминале команду:

```dash
curl http://ваш_ip/v1/zouzoubliques
```  

### Если в ответ не получены данные о зарегистрированном зюзюблике - свяжитесь с автором этой инструкции.