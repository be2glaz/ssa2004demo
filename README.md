# ssa2004demo

### Общая информация

<details>
<summary>ТЫКНИ</summary>

Оценочные материалы демонстрационного экзамена 2024 года 09.02.06 «Сетевое и системное администрирование»

    https://bom.firpo.ru/

КОД 09.02.06-1-2024 Том 1

    https://bom.firpo.ru/file/9791/%D0%9A%D0%9E%D0%94%2009.02.06-1-2024%20%D0%A2%D0%BE%D0%BC%201.pdf

[Задание Модуль 1](http://wiki.prcit.ru/Demo-2024/%D0%9C%D0%BE%D0%B4%D1%83%D0%BB%D1%8C-1)

В данном варианте решения предполагается использовать RedOS 7.3.4 Сервер минимальный и RedOS 7.3.4 Рабочая станция

    https://files.red-soft.ru/redos/7.3/x86_64/iso/redos-MUROM-7.3.4-20231220.0-Everything-x86_64-DVD1.iso
калькулятор ipv4

    https://ipmeter.ru/
калькулятор ipv6

    https://www.coderstool.com/ipv6-subnet-calculator
drawio

    https://app.diagrams.net/
    
)

</details>

# КАРТИНОЧКИИИ

<details>
<summary>ТЫКНИ</summary>

![topology](https://github.com/be2glaz/ssa2004demo/assets/89695370/54472aa7-2573-4f55-b219-bf314e30f1ec)

![tab_1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a48d854b-7284-4f67-8318-ce1c1a6ea22d)

![ip_adr_tabl(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)

![l3_topologiya_(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/3a2e1161-db7c-4627-8191-98a602cd43ef)

</details>

## 1. Базовая настройка

<details>
<summary>ТЫКНИ</summary>
    
![topology](https://github.com/be2glaz/ssa2004demo/assets/89695370/54472aa7-2573-4f55-b219-bf314e30f1ec)

1. Выполните базовую настройку всех устройств:
a. Присвоить имена в соответствии с топологией
b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.
c. Пул адресов для сети офиса BRANCH - не более 16
d. Пул адресов для сети офиса HQ - не более 64

![tab_1](https://github.com/be2glaz/ssa2004demo/assets/89695370/a48d854b-7284-4f67-8318-ce1c1a6ea22d)


![ip_adr_tabl(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/91c000bb-9a96-4017-8ee9-86e59870074c)


а. Присвоить имена в соответствии с топологией
Имена устройств (hostname) – прописывать строчными символами (маленькими буквами)

    [root@localhost ~]# hostnamectl set-hostname <NAME>
    [root@localhost ~]# exec bash

NAME - имя устройства

exec bash — перезапуск оболочки bash для отображения нового хостнейма

Для устройств BR-SRV и CLI желательно сразу установить полное доменное имя. Потребуется для ввода этих машин в домен во второй части задания.

> Например:
> - ISP: isp
> - CLI: cli.hq.work
> - HQ-R: hq-r.hq.work
> - HQ-SRV: hq-srv.hq.work
> - BR-R: br-r.branch.work
> - BR-SRV: br-srv.branch.work

Пример:

![1-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/cb447ca2-2e79-496b-8643-97fe1d349fe8)


b. Рассчитать IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.

c. Пул адресов для сети офиса BRANCH - не более 16


> [!WARNING]
> 
> - Для пула адресов IPv4 не более 16 - маска подсети /28
> - Для пула адресов IPv6 не более 16 - длина префикса /124



d. Пул адресов для сети офиса HQ - не более 64


> [!WARNING]
> 
>  - Для пула адресов IPv4 не более 64 - маска подсети /26
>  - Для пула адресов IPv6 не более 64 - длина префикса /122

</details>

### Настройка сетевых интерфейсов

<details>
<summary>ТЫКНИ</summary>

**ISP**
Определяемся имена интерфейсов и какой интерфейс в какую сторону смотрит

Выводим информацию о сетевых интерфейсах:

    # ip -c a

![1-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/cf26d254-96d5-495e-9c99-ebf201d9a5c4)

Открываем настройки виртуальной машины

Выбираем необходимую виртуальную машину
Выбираем Оборудование
Смотрим MAC-адрес сетевых интерфейсов, и запоминаем их (лучше записать на черновик)

![1-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/a52f6ddf-f930-466e-87cd-d0426988931a)

С помощью утилиты ```nmtui``` задаем IP адреса сетевым интерфейсам

**Результаты настройки сетевых интерфейсов**

**ISP**

В данном примере получаем:

- ens18 – WAN интерфейс (в Интернет);
- ens19 - интерфейс в сторону офиса HQ;
- ens20 - интерфейс в сторону CLI;
- ens21 - интерфейс в сторону офиса Branch;

![1-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/21e62765-542e-40b0-88b5-ddd9f1971ed0)

**HQ-R**

В данном примере для HQ-R:

- ens18 - интерфейс в сторону ISP;
- ens19 - интерфейс в строну офиса HQ;
- ens20 - интерфейс в сторону CLI (временное подключение) ;

![1-5](https://github.com/be2glaz/ssa2004demo/assets/89695370/128d0d00-366b-4d76-ba91-df4899399c8e)


**HQ-SRV**
Получает IP адрес по DHCP от HQ-R. Настройка описана ниже.

В данном примере для HQ-SRV:

- ens18 - интерфейс в строну офиса HQ;

> Режим КОНФИГУРАЦИЯ IPv4 <Автоматически>
> 
> Изменяем режим КОНФИГУРАЦИЯ IPv6 с <Автоматически> на <Автоматически (только DHCP)>


**BR-R**

В данном примере для HQ-R:

- ens18 - интерфейс в сторону ISP;
- ens19 - интерфейс в строну офиса Branch;

![1-6](https://github.com/be2glaz/ssa2004demo/assets/89695370/9a0c8268-41be-4698-9981-5a78c95dec27)


**BR-SRV**

BR-SRV - 1 интерфейс в сторону BR-R

![1-7](https://github.com/be2glaz/ssa2004demo/assets/89695370/9ede3139-bc65-4611-b36f-9f2cd51a1039)


**CLI**

Настройка интерфейса CLI_ISP

![1-8](https://github.com/be2glaz/ssa2004demo/assets/89695370/ec1ce4b0-79a8-407f-9511-c033ea9e6ef4)

![1-9](https://github.com/be2glaz/ssa2004demo/assets/89695370/23717674-91a4-4d38-8e5f-fe7bfa1324af)

Настройка интерфейса HQ-R_CLI (временное соединение)

Настраивается аналогично CLI_ISP

![1-10](https://github.com/be2glaz/ssa2004demo/assets/89695370/55685e14-7bf5-4134-958f-1d17cda77e41)

</details>

## Настройка доступа в интернет

### Маршрутизация транзитных IP-пакетов

<details>
<summary>ТЫКНИ</summary>

> На устройствах ISP, HQ-R, BR-R необходимо включить пересылку пакетов между интерфейсами - forwarding

Чтобы включить пересылку пакетов между интерфейсами, необходимо отредактировать файл sysctl.conf

    # nano /etc/sysctl.conf
В данном файле прописываем следующие строки:

    net.ipv4.ip_forward=1
    net.ipv6.conf.all.forwarding=1

После необходимо применить внесенные изменения:

    # sysctl -p

> Необходимо предоставить доступ в сеть Интернет для всех устройств предложенных в демо-экзамене для установки необходимых пакетов. Для этого необходимо настроить Nftables на устройствах ISP, HQ-R и BR-R

> Nftables - подсистема ядра Linux, обеспечивающая фильтрацию и классификацию сетевых пакетов/датаграмм/кадров.


#### Настройка nftables на ISP

> Данная настройка позволит получить доступ к сети Интернет с HQ-R и BR-R

Установка nftables

Перед установкой необходимо убедиться что имеется доступ в интернет с ВМ ISP

    ping -c4 ya.ru
Если ping проходит успешно то устанавливаем nftables

    # dnf install -y nftables

#### Настройка nftables
По умолчанию создаются несколько примеров файлов для работы с nftables в директории /etc/ nftables/.

Настройка с использованием собственного файла настроек

Можно не использовать ни один из файлов примеров, а написать свой.

Создаем и открываем файл

    # nano /etc/nftables/isp.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
где ```ens18``` - публичный интерфейс ISP (смотрящий в Интернет)

Затем необходимо включить использование данного файла в ```sysconfig``` , по умолчанию ```nftables``` не читает ни один из конфигурационных файлов в ```/etc/nftables```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/isp.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables
> При успешной и правильной настройке машины ```HQ-R``` и ```BR-R``` получат выход в Интернет

> На устройствах ```HQ-R``` и ```BR-R``` необходимо произвести настройку ```Nftables``` аналогичным способом для доступа HQ-SRV и BR-SRV к сети Интернет

### Настройка nftables на HQ-R
Установка nftables

    # dnf install -y nftables
Создаем и открываем фалй

    # nano /etc/nftables/hq-r.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
Включаем использование данного файла в ```sysconfig```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/hq-r.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables

### Настройка nftables на BR-R
Установка nftables

    # dnf install -y nftables
Создаем и открываем файл

    # nano /etc/nftables/br-r.nft
Прописываем следующие строки

    table inet my_nat {
            chain my_masquerade {
            type nat hook postrouting priority srcnat;
            oifname "ens18" masquerade
            }
    }
Включаем использование данного файла в ```sysconfig```

    # nano /etc/sysconfig/nftables.conf
Ниже строки начинающейся на ```include```, прописываем строку

    include "/etc/nftables/br-r.nft"
Запуск и добавление в автозагрузку сервиса ```nftables```

    # systemctl enable --now nftables



</details>

## 2. Настроить внутреннюю динамическую маршрутизацию по средствам FRR. Выбрать и обосновать выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.

<details>
<summary>ТЫКНИ</summary>

##### a. Составьте топологию сети L3.
### Решение

> Маршрутизация внешних сетей по заданию не описана, следовательно, на HQ-R и BR-R достаточно настроить статическую маршрутизацию.
> 
> При настройке IP-адресации в качестве шлюза задать соответствующие адреса маршрутизатора ISP

> Необходимо связать HQ-R и BR-R туннелем. Обмен между внутренними сетями должен происходить строго между маршрутизаторами HQ и BRANCH. ISP не должен иметь к ним прямого доступа.
> 
> Достаточно реализовать простой GRE туннель

#### GRE-туннель между HQ-R и BR-R
> Имена tun0, gre0 и sit0 являются зарезервированными в iproute2 («base devices») и имеют особое поведение.

##### Настройка HQ-R
Так как в РЕД ОС используется NetworkManager - следовательно переходим в nmtui:

    # nmtui
**Производим настройку**
- Выбираем «Изменить подключение»
- Выбираем «Добавить»
- Выбираем «IP-туннель
- Задаём понятные имена «Имя профиля» и «Устройство»
- «Режим работы» выбираем «GRE»
- «Родительский» указываем интерфейс в сторону ISP (ens18)
- Задаём «Локальный IP» (IP на интерфейсе HQ-R в сторону IPS)
- Задаём «Удалённый IP» (IP на интерфейсе BR-R в сторону ISP)
- Переходим к «КОНФИГУРАЦИЯ IPv4»
- Задаём адрес IPv4 для туннеля
- Переходим к «КОНФИГУРАЦИЯ IPv6»
- Задаём адрес IPv6 для туннеля
- Активируем интерфейс tun1

![gre-gif](https://github.com/be2glaz/ssa2004demo/assets/89695370/7a5f7735-22b5-40bc-b905-ad2c73826e6a)

> Для корректной работы протокола динамической маршрутизации требуется увеличить параметр TTL на интерфейсе туннеля:

    # nmcli connection modify tun1 ip-tunnel.ttl 64
Проверяем:

    ip -c a

![2-1](https://github.com/be2glaz/ssa2004demo/assets/89695370/bf42efae-339f-4cf2-9d5c-01a02deed555)

##### Настройка BR-R
Настройка GRE – туннеля на BR-R производится аналогично HQ-R

    # nmtui
**Производим настройку**
- Выбираем «Изменить подключение»
- Выбираем «Добавить»
- Выбираем «IP-туннель
- Задаём понятные имена «Имя профиля» и «Устройство»
- «Режим работы» выбираем «GRE»
- «Родительский» указываем интерфейс в сторону ISP (ens18)
- Задаём «Локальный IP» (IP на интерфейсе BR-R в сторону IPS)
- Задаём «Удалённый IP» (IP на интерфейсе HQ-R в сторону ISP)
- Переходим к «КОНФИГУРАЦИЯ IPv4»
- Задаём адрес IPv4 для туннеля
- Переходим к «КОНФИГУРАЦИЯ IPv6»
- Задаём адрес IPv6 для туннеля
- Активируем интерфейс tun1

> Был создан новый виртуальный интерфейс (туннель) для прямого взаимодействия устройств HQ-R и BR-R. Они будут напрямую обмениваться маршрутами внутренних сетей HQ и BRANCH через это соединение.

Проверяем

<details>
<summary>ТЫКНИ</summary>

HQ-R

![2-2](https://github.com/be2glaz/ssa2004demo/assets/89695370/dbc71991-395b-477c-84ab-fd8a0b6ca039)

BR-R

![2-3](https://github.com/be2glaz/ssa2004demo/assets/89695370/7db6d99e-24b9-433e-800a-c077e5e35fce)

</details>

### Настройка динамической (внутренней) маршрутизации средствами FRR
#### Настройка на HQ-R
Установка пакет frr

    # dnf install -y frr
Для настройки внутренней динамической маршрутизации для IPv4 и IPv6 будет использован протокол ```OSPFv2``` и ```OSPFv3```

Для настройки ```ospf``` необходимо включить соответствующий демон в конфигурации ```/etc/frr/daemons```

    # nano /etc/frr/daemons
В конфигурационном файле ```/etc/frr/daemons``` необходимо активировать выбранный протокол для дальнейшей реализации его настройки:

> ```ospfd = yes``` - для OSPFv2 (IPv4)
>
> ```ospf6d = yes``` - для OSPFv3 (IPv3)

Включаем и добавляем в автозагрузку службу FRR

    # systemctl enable --now frr
Переходим в интерфейс управление симуляцией FRR при помощи vtysh (аналог cisco)

    # vtysh
Настройки OSPFv2 и OSPFv3 на HQ-R

![2-4](https://github.com/be2glaz/ssa2004demo/assets/89695370/a41c38ba-ef9b-4075-ae7b-44d07cf26911)



> OSPFv2
>
> conf t или configure terminal - вход в режим глобальной конфигурации
> router ospf - переход в режим конфигурации OSPFv2
> passive-interface default - перевод всех интерфейсов в пассивный режим
> network - объявляем локальную сеть офиса HQ и сеть (GRE-туннеля)
> exit - выход и режима конфигурации OSPFv2
> туннельный интерфейс tun1 делаем активным, для устанавления соседства с BR-R и обмена внутренними маршрутами
> no ip ospf passive - перевод интерфейса tun1 в активный режим
> do write - сохраняем текущую конфигурацию

> OSPFv3 - ipv6
> 
> router ospf6 - переход в режим конфигурации OSPFv3
> ospf6 router-id - назначение номера router-id
> сети интерфейсов tun1 и enp0s3 добавляем в конфигурацию OSPFv3
> do write - сохраняем текущую конфигурацию

Перезапускаем frr

    # systemctl restart frr
Посмотреть текущую конфигурацию можно с помощью следующих команд

    #  vtysh
    
    # show running-config

#### Настройка на BR-R
Настройки ```OSPFv2``` и ```OSPFv3``` на BR-R аналогичны HQ-R

Необходимо изменить

- объявляемые сети в OSPFv2;
- router-id в OSPFv3
Настройки OSPFv2 и OSPFv3 на BR-R

![2-5](https://github.com/be2glaz/ssa2004demo/assets/89695370/e3c1ea13-8cac-4769-af4b-4a6e6e3454bd)

Посмотреть текущую конфигурацию можно с помощью следующих команд

    #  vtysh
    
    # show running-config


### Проверка

<details>
<summary>ТЫКНИ</summary>

Получить информацию о соседях и установленных отношениях соседства.

    // для IPv4
    # show ip ospf neighbor

    // для IP6
    # show ipv6 ospf6 neighbor
Показать маршруты, полученные от процесса OSPF.

    // для IPv4
    # show ip route ospf
    
    // для IPv6
    # show ipv6 route ospf6
HQ-R

![2-6](https://github.com/be2glaz/ssa2004demo/assets/89695370/737aa0d8-12be-454a-83e8-a13ab4da85c2)

BR-R

![2-7](https://github.com/be2glaz/ssa2004demo/assets/89695370/6364c9e1-0848-4384-8aaf-3fd781452253)


</details>

#### Топология L3

![l3_topologiya_(2)](https://github.com/be2glaz/ssa2004demo/assets/89695370/48d7c679-dafd-45fe-8bd7-9f8c3b7e2df0)

</details>


## 1.3.Настройте автоматическое распределение IP-адресов на роутере HQ-R

### Задание
#### Настройте автоматическое распределение IP-адресов на роутере HQ-R.
- a. Учтите, что у сервера должен быть зарезервирован адрес.

<details>
<summary>ТЫКНИ</summary>

### Решение
#### Настройка DHCP на HQ-R для IPv4

Установка DHCP

    # dnf install  dhcp-server
> Настройки для диапазона адресов IPv4 производятся в файле /etc/dhcp/dhcpd.conf. Пример данного файла можно посмотреть в файле /usr/share/doc/dhcp-server/dhcpd.conf.example.

Открываем файл конфигурации

    # nano /etc/dhcp/dhcpd.conf
Подсети обозначаются блоками, пример такого блока представлен ниже:

    subnet 172.16.100.0 netmask 255.255.255.192 {
      range 172.16.100.2 172.16.100.62;
      option routers 172.16.100.1;
      default-lease-time 600;
      max-lease-time 7200;
    }
где

- ```subnet``` - обозначает сеть, в области которой будет работать данная группа настроек;
- ```range``` — диапазон, из которого будут браться IP-адреса;
- ```option routers``` — шлюз по умолчанию;
- ```default-lease-time```, ```max-lease-time``` — время и максимальное время в секундах, на которое клиент получит адрес, по его истечению будет выполнено продление срока.

**Резервирование ip-адреса за клиентом**
Хосту с именем ```HQ-SRV``` , у которого сетевая карта имеет MAC ```ff:ff:ff:ff:ff:ff``` зарезервируем адрес ```172.16.100.2```.

    host HQ-SRV {
            hardware ethernet ff:ff:ff:ff:ff:ff;
            fixed-address 172.16.100.2;
    }
```ff:ff:ff:ff:ff:ff``` - mac адрес интерфейса которому будет выдан статический ip-адрес

Выбираем интерфейс, для которого будет работать DHCP сервер

Открываем файл конфигурации

    # nano /etc/sysconfig/dhcpd
Добавляем в него следующее:

    DHCPDARGS=ens19
где

```ens19``` - интерфейс смотрящий в сторону HQ-SRV
Запускаем и добавляем в автозагрузку службу dhcpd (для IPv4):

    # systemctl enable --now dhcpd

#### Проверка на HQ-SRV
Открываем на HQ-SRV настройку сетевых интерфейсов

    # nmtui
Настраиваем интерфейс на автоматическое получение адресов

![2-8](https://github.com/be2glaz/ssa2004demo/assets/89695370/7ccbe25e-0aef-4292-a184-cabddc8c81f0)

Перезагружаем интерфейс и убеждаемся в работоспособности DHCP сервера

![2-9](https://github.com/be2glaz/ssa2004demo/assets/89695370/4d0118db-3ddb-4c93-ae1d-aad6c427fa86)


#### Настройка DHCP на HQ-R для IPv6
> Настройки для диапазона адресов IPv6 производятся в файле ```/etc/dhcp/dhcpd6.conf```. Пример данного файла можно посмотреть в файле ```/usr/share/doc/dhcp-server/dhcpd6.conf.example```.

Для облегчения создания конфигурационного файла для DHCPv6

- Создаем резервную копию файла ```/etc/dhcp/dhcpd6.conf``` переименовав его
- Копируем файл ```/usr/share/doc/dhcp-server/dhcpd6.conf.example``` в директорию ```/etc/dhcp/``` с именем ```dhcpd6.conf```

![2-10](https://github.com/be2glaz/ssa2004demo/assets/89695370/c4c8e5d6-b4d4-47f7-b718-5f3e29b7babf)

Открываем на редактирование файл конфигурации DHCPv6

    # nano /etc/dhcp/dhcpd6.conf
Приводим файл к следующему виду удалив строки

> Вы можете использовать клавиши Ctrl + K, которые вырезают всю строку






</details>
