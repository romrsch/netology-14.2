# Домашнее задание к занятию "14.2 Синхронизация секретов с внешними сервисами. Vault"

## Задача 1: Работа с модулем Vault

Запустить модуль Vault конфигураций через утилиту kubectl в установленном minikube

```
kubectl apply -f 14.2/vault-pod.yml
```
***Ответ:***
![Alt text](https://i.ibb.co/S02PnVJ/Screenshot-6.jpg)

Получить значение внутреннего IP пода

```
kubectl get pod 14.2-netology-vault -o json | jq -c '.status.podIPs'
```
***Ответ:***
![Alt text](https://i.ibb.co/42bCj3q/Screenshot-7.jpg)


Запустить второй модуль (под) для использования в качестве клиента

```
kubectl run -i --tty fedora --image=fedora --restart=Never -- sh
```
![Alt text](https://i.ibb.co/g97YdyX/Screenshot-8.jpg)
![Alt text](https://i.ibb.co/Cm0fhnN/Screenshot-12.jpg)
Установить дополнительные пакеты
```
dnf -y install pip
pip install hvac
```
***Ответ:***
![Alt test](https://i.ibb.co/8DQC33K/Screenshot-9.jpg)
![Alt test](https://i.ibb.co/cTjzQ3m/Screenshot-10.jpg)

Запустить интепретатор Python и выполнить следующий код, предварительно
поменяв IP и токен

```
import hvac
client = hvac.Client(
    url='http://10.10.133.71:8200',
    token='aiphohTaa0eeHei'
)
client.is_authenticated()

# Пишем секрет
client.secrets.kv.v2.create_or_update_secret(
    path='hvac',
    secret=dict(netology='Big secret!!!'),
)

# Читаем секрет
client.secrets.kv.v2.read_secret_version(
    path='hvac',
)
```
***Ответ:***

![Alt text](https://i.ibb.co/0MXZWK8/Screenshot-13.jpg)


---
