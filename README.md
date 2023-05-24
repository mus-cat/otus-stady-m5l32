# Routing
## Задание выполнялось с использованием VirtualBox 6.1.40, использовался vagrant с box-образом generic/debian11

## Задание:
- Поднять OSPF между 3 ВМ на базе Quagga (FRR);
- Изобразить ассиметричный роутинг;
- Сделать один из линков "дорогим", но что бы при этом роутинг был симметричным

## Решение
### Поднять OSPF между 3 ВМ на базе Quagga (FRR) 
В результате работы **Vagrant** будет построена следующая схема сети  
!["Схема сети"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/Network.png)  

После обмена маршрутной информацией таблицы маршрутизации на `Router1` будет следующая :
!["Routing table"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/router1_routingTable.png)

### Изобразить ассиметричный роутинг
Меняем "стоимость" интерфейса на `Router1` (задаем значение 1000) соединяющего его с `Router2`. В результатет таблица маршрутизации изменится:
!["Routing table after change interface cost #1"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/Router1_changeOSPFcost.png)

Проверить, что трафик ассиметричен, можно выполнив команду **traceroute** на `Router1`и `Router2`
!["Traceroute 1"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/TracerouteResult1.png)

### Сделать один из линков "дорогим", но что бы при этом роутинг был симметричным
Для достижения указаной цели изменим стоимость интерфейса на `Router3` соединяющего его с `Router2`, выставив значение в 1000. В результате получим следующую таблицу маршрутизации на `Router1`.
!["Routing table after change interface cost #2"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/router1_chOSPFcostR3.png)

Проверить, что трафик симетричен, может опять же выполнив команду **traceroute** на `Router1`и `Router2`
!["Traceroute 2"](https://github.com/mus-cat/otus-stady-m5l32/blob/main/pic/TracerouteResult2.png)

### Файлы:
- **Vagrant** - файл для создания ВМ и запуска ansible
- **router.yml** - ansible playbook для настройки програмных маршрутизаторов на базе **FRR**
- **frr.conf.j2** - шаблон для настройки маршрутизаторов
