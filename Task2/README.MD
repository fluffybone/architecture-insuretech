 ### Установка kubectl (Интерфейс командной строки Kubernetes)
 `curl.exe -LO "https://dl.k8s.io/release/$(curl.exe -L -s https://dl.k8s.io/release/stable.txt)/bin/windows/amd64/kubectl.exe"`

 Добавьте kubectl в переменную среды PATH:
Переместите скачанный файл kubectl.exe в директорию, которая уже находится в вашем PATH (например, C:\Windows\System32 или создайте новую папку, например, C:\K8S\tools, и добавьте ее в PATH).

`kubectl version --client`

### Установка Minikube

Minikube — это инструмент, который позволяет запускать одноузловой кластер Kubernetes локально.

`curl.exe -LO https://storage.googleapis.com/minikube/releases/latest/minikube-installer.exe`

`minikube version`

#### Запуск кластера Minikube

`minikube start --driver=docker`

`minikube status` - Проверка статус кластера
`kubectl get nodes` - Проверка соединение с кластером

### Активация metrics-server

`minikube addons enable metrics-server`

#### Проверка работы metrics-server

После активации может потребоваться некоторое время, чтобы поды metrics-server запустились. Вы можете проверить статус подов в пространстве имен kube-system:

`minikube kubectl -- get pods -n kube-system | Select-String "metrics-server"`
 статус пода должен  `Running`.

Проверка функциональности

#### Показать использование ресурсов узлами (нод)
`minikube kubectl -- top node`

## Развертывание приложения

### Создать файл манифеста (Deployment) в формате YAML, сохранить его локально, а затем применить к кластеру Minikube.

[scaletestapp-deployment.yaml](Task2\scaletestapp-deployment.yaml)
### Применить файл в minikube
`minikube kubectl -- apply -f D:\project\architecture-insuretech\Task2\scaletestapp-deployment.yaml`
#### Проверка Deployment:
`minikube kubectl -- get deployment scaletest-app-deployment`
#### Проверка Pod:
`minikube kubectl -- get pods | Select-String "scaletestapp"`


## Создание Service
[scaletestapp-service.yaml](Task2\scaletestapp-service.yaml)

### Применение Service
`minikube kubectl -- apply -f D:\project\architecture-insuretech\Task2\scaletestapp-service.yaml`
### Проверка Service
`minikube service scaletest-app-service --url`


##  Настройка Horizontal Pod Autoscaler (HPA)
[scaletestapp-hpa.yaml](Task2\scaletestapp-hpa.yaml)
Применение
`minikube kubectl -- apply -f D:\project\architecture-insuretech\Task2\scaletestapp-hpa.yaml`
Проверка
`minikube kubectl -- get hpa`

## Установка Locust (`pip install locust`)
[Тест](Task2\locustfile.py)

