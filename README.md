o# **Архитектура сетей.**

## **Homework**

**Построить следующую архитектуру**

Сеть office1

  - 192.168.2.0/26   - dev
  - 192.168.2.64/26  - test servers
  - 192.168.2.128/26 - managers
  - 192.168.2.192/26 - office hardware

Сеть office2

  - 192.168.1.0/25   - dev
  - 192.168.1.128/26 - test servers
  - 192.168.1.192/26 - office hardware

Сеть central

  - 192.168.0.0/28  - directors
  - 192.168.0.32/28 - office hardware
  - 192.168.0.64/26 - wifi

Office1 ---\
      -----> Central --IRouter --> internet
Office2----/

Итого должны получится следующие сервера

  - inetRouter
  - centralRouter
  - office1Router
  - office2Router
  - centralServer
  - office1Server
  - office2Server

Теоретическая часть

  - Найти свободные подсети
  - Посчитать сколько узлов в каждой подсети, включая свободные
  - Указать broadcast адрес для каждой подсети
  - проверить нет ли ошибок при разбиении

Практическая часть

  - Соединить офисы в сеть согласно схеме и настроить роутинг
  - Все сервера и роутеры должны ходить в инет черз inetRouter
  - Все сервера должны видеть друг друга
  - у всех новых серверов отключить дефолт на нат (eth0), который вагрант поднимает для связи
  - при нехватке сетевых интервейсов добавить по несколько адресов на интерфейс

# **Теоретическая часть**

**Сначала посчитаем сколько узлов в каждой подсети и сразу станет понятно какие подсети свободные**

| Сеть     |    Название     |     Подсеть      |   Hostmin     | Hostmax       |  Broadcast    | Hosts |
|----------|-----------------|:-----------------|:--------------|:--------------|:--------------|:-----:|
| office1  | dev             | 192.168.2.0/26   | 192.168.2.1   | 192.168.2.62  | 192.168.2.63  | 62    |
|          | test servers    | 192.168.2.64/26  | 192.168.2.65  | 192.168.2.126 | 192.168.2.127 | 62    |
|          | managers        | 192.168.2.128/26 | 192.168.2.129 | 192.168.2.190 | 192.168.2.191 | 62    |
|          | office hardware | 192.168.2.192/26 | 192.168.2.193 | 192.168.2.254 | 192.168.2.255 | 62    |
| office2  | dev             | 192.168.1.0/25   | 192.168.1.1   | 192.168.1.126 | 192.168.1.127 | 126   |
|          | test servers    | 192.168.1.128/26 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 | 62    |
|          | office hardware | 192.168.1.192/26 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 | 62    |
| central  | directors       | 192.168.0.0/28   | 192.168.0.1   | 192.168.0.1   | 192.168.0.15  | 14    |
|          | office hardware | 192.168.0.32/28  | 192.168.0.33  | 192.168.0.46  | 192.168.0.47  | 14    |
|          | wifi            | 192.168.0.64/26  | 192.168.0.65  | 192.168.0.126 | 192.168.0.127 | 62    |

**Исходя из таблицы заметно, что в office1 и office2 использованы все подсети, а вот в сети central остались не задествованы:**

| Сеть     |     Подсеть      |   Hostmin     | Hostmax       |  Broadcast    | Hosts |
|----------|:-----------------|:--------------|:--------------|:--------------|:-----:|
| central  | 192.168.0.16/28  | 192.168.0.17  | 192.168.0.30  | 192.168.0.31  | 14    |
|          | 192.168.0.48/28  | 192.168.0.49  | 192.168.0.62  | 192.168.0.63  | 14    |
|          | 192.168.0.128/25 | 192.168.0.129 | 192.168.0.254 | 192.168.0.255 | 126   |

Ошибки при разбиении не обнаружены.

# **Практическая часть**

Схема сети:

office1server ---> office1router|
                                 
                                 \

                                   ---> centralrouter ---> inetRouter ---> internet
                                
                                 /         \

office2server ---> office2router|         centralserver
eef
