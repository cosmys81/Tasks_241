# Networks (Сети)

## **1. Выведите список интерфейсов, какими способами можно это сделать?**

```bash
ip a
```

Вывод: 

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:b1:f5:22 brd ff:ff:ff:ff:ff:ff
    inet 10.0.3.214/24 brd 10.0.3.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feb1:f522/64 scope link
       valid_lft forever preferred_lft forever
```

---

```bash
netstat -i
```

Вывод:

```bash
Kernel Interface table
Iface       MTU Met    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500   0  5344392      0      1      0    47637      0      0      0 BMRU
lo        65536   0       22      0      0      0       22      0      0      0 LRU
```

---

```bash
ls /sys/class/net/
```

Вывод:

```bash
eth0  lo
```

---

```bash
cat /proc/net/dev
```

Вывод:

```bash
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
    lo:    1980      22    0    0    0     0          0         0     1980      22    0    0    0     0       0          0
  eth0: 467657548 5346152    0    1    0     0          0         0  3721270   47694    0    0    0     0       0          0
```

---

```bash
ifconfig
```

Вывод:

```bash
eth0      Link encap:Ethernet  HWaddr BC:24:11:B1:F5:22
          inet addr:10.0.3.214  Bcast:10.0.3.255  Mask:255.255.255.0
          inet6 addr: fe80::be24:11ff:feb1:f522/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:5440685 errors:0 dropped:1 overruns:0 frame:0
          TX packets:48508 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:473678259 (451.7 MiB)  TX bytes:3794639 (3.6 MiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:22 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1980 (1.9 KiB)  TX bytes:1980 (1.9 KiB)
```

## **2. Попробуйте изменить ip адрес**

```bash
ifconfig eth0 192.168.0.1 netmask 255.255.255.0
```

Вывод:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:11:01:f3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.1/24 brd 192.168.0.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fd00::a00:27ff:fe11:1f3/64 scope global dynamic mngtmpaddr
       valid_lft 86009sec preferred_lft 14009sec
    inet6 fe80::a00:27ff:fe11:1f3/64 scope link
       valid_lft forever preferred_lft forever 
```


## **3. Попробуйте добавить несколько ip адресов на сетевую карту**

```bash
sudo ip addr add 192.168.1.101/24 dev eth0
sudo ip addr add 192.168.1.102/24 dev eth0
sudo ip addr add 192.168.1.103/24 dev eth0
sudo ip addr add 192.168.1.104/24 dev eth0
```

Вывод:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:b1:f5:22 brd ff:ff:ff:ff:ff:ff
    inet 10.0.3.214/24 brd 10.0.3.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 192.168.1.100/24 scope global eth0
       valid_lft forever preferred_lft forever
    inet 192.168.1.101/24 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet 192.168.1.102/24 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet 192.168.1.103/24 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet 192.168.1.104/24 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feb1:f522/64 scope link
       valid_lft forever preferred_lft forever
```

## **4. Выведите список маршрутов**

```bash
ip route show
```

Вывод:

```bash
default via 10.0.3.1 dev eth0 proto static
10.0.3.0/24 dev eth0 proto kernel scope link src 10.0.3.214
```

## **5. Выведите arp таблицу**

На Windows:

```bash
arp -a
```

Вывод:

```bash
Interface: 192.168.56.1 --- 0x9
  Internet Address      Physical Address      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.2             01-00-5e-00-00-02     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static

Interface: 192.168.0.104 --- 0xd
  Internet Address      Physical Address      Type
  192.168.0.1           c4-6e-1f-fc-e9-90     dynamic
  192.168.0.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.2             01-00-5e-00-00-02     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  224.0.0.253           01-00-5e-00-00-fd     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static
```

На линуксе:

```bash
ip neigh show
```

Вывод:

```bash
10.0.3.2 dev eth0  FAILED
10.0.3.1 dev eth0 lladdr 52:ff:20:7b:bf:42 REACHABLE
```

## **6. Что такое ip адрес?**

IP-адрес (Internet Protocol Address) — это числовой идентификатор, который используется для обозначения устройств в сети и обеспечения маршрутизации данных между ними.

**Типы:**

- **IPv4** — адреса состоят из 4 октетов (байтов), разделённых точками, например: `192.168.1.1`.
    - Диапазон: от `0.0.0.0` до `255.255.255.255`.
    - Пример адресов:
        - Частные: `192.168.0.0/16`, `10.0.0.0/8`, `172.16.0.0/12`.
        - Публичные: все остальные (например, `8.8.8.8` — DNS Google).
- **IPv6** — более новый стандарт с адресами длиной 128 бит, записанными в формате 8 групп по 4 шестнадцатеричных числа, например: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.

**Принципы работы:**

1. Каждый узел в сети получает уникальный IP-адрес.
2. IP-адрес используется для доставки данных от отправителя к получателю.
3. IP-адреса включают:
    - **Сетевую часть** (идентификатор сети).
    - **Хостовую часть** (идентификатор устройства).

## **7. Для чего нужны маршруты?**

Маршруты (Routing) определяют, по каким путям сетевые пакеты передаются между источником и получателем через сети.

**Задачи маршрутизации:**

- Определить, куда отправить данные (локально, на шлюз или в другую сеть).
- Управлять трафиком между локальной сетью и внешним интернетом.

**Типы маршрутов:**

1. **Локальные маршруты** — для устройств в той же подсети.
2. **Шлюз по умолчанию (Default Gateway)** — маршрут для всех адресов, не принадлежащих локальной сети.
3. **Статические маршруты** — задаются вручную для передачи трафика через определённые пути.
4. **Динамические маршруты** — автоматически управляются протоколами маршрутизации (например, OSPF, BGP).

## **8. Что за протокол arp?**

**ARP (Address Resolution Protocol)** — это сетевой протокол, используемый для определения MAC-адреса устройства по его IP-адресу в пределах локальной сети.

**Работает ARP так:**

1. Устройство отправляет ARP-запрос (широковещательный) с вопросом: "Кто владеет IP-адресом 192.168.1.2?".
2. Устройство с этим IP отвечает своим MAC-адресом.
3. Результат сохраняется в ARP-таблице для ускорения будущих запросов.

## **9. Что такое dhcp?**

**DHCP (Dynamic Host Configuration Protocol)** — протокол для автоматического назначения сетевых параметров устройствам в сети.

**Функции DHCP:**

- Назначение IP-адресов.
- Выдача информации о DNS-серверах, шлюзах, масках подсети.

**Пример работы DHCP:**

1. Устройство отправляет запрос DHCP Discover для получения параметров сети.
2. DHCP-сервер отвечает с параметрами (например, IP: `192.168.1.100`, шлюз: `192.168.1.1`).

## **10. Что такое dns?**

**DNS (Domain Name System)** — система, преобразующая доменные имена в IP-адреса и наоборот.

**Принцип работы:**

1. Пользователь вводит доменное имя (например, `google.com`).
2. DNS-сервер ищет соответствующий IP-адрес.
3. Устройство подключается к серверу через найденный IP.

**Типы DNS-записей:**

- **A (Address Record)** — связывает домен с IPv4-адресом.
- **AAAA** — связывает домен с IPv6-адресом.
- **CNAME** — псевдоним для домена.
- **MX** — записи для почтовых серверов.

## **11. Как называется один из протоколов синхронизации времени?**

**NTP (Network Time Protocol)** — протокол, используемый для синхронизации времени между устройствами в сети.

- Работает по модели клиент-сервер.
- Поддерживает точность до миллисекунд.
- Сервисы, предоставляющие NTP:
    - `pool.ntp.org`
    - Локальные NTP-серверы.

## **12. Что такое широковещательный запрос, зачем он нужен?**

Широковещательный запрос — это сообщение, отправляемое всем устройствам в одной сети.

**Пример использования:**

- Протокол ARP использует широковещательные запросы для поиска MAC-адресов.
- DHCP использует широковещание, чтобы найти DHCP-сервер.

**Зачем нужен:**

- Для связи с устройствами, IP-адрес которых неизвестен.
- Для уведомления всех узлов в сети.

## **13. Какой адрес является широковещательным?**

Широковещательный адрес в IPv4 — это последний адрес в подсети.

- Пример: для сети `192.168.1.0/24` широковещательный адрес — `192.168.1.255`.

В IPv6 широковещательных запросов нет, вместо них используется **мультикаст** (например, `ff02::1`).

## **14. Какие ещё параметры можно задать сетевой карте?**

- IP-адрес.
- Маска подсети.
- Шлюз по умолчанию.
- DNS-серверы.
- MTU (Maximum Transmission Unit).
- MAC-адрес.
- Метрика интерфейса.

## **15. Что такое маска подсети? зачем она нужна?**

**Маска подсети** определяет, какая часть IP-адреса указывает на сеть, а какая — на хост.

**Пример:**

- Маска: `255.255.255.0` или `/24`.
- IP-адрес: `192.168.1.100`.
    - Сеть: `192.168.1.0`.
    - Устройства: `192.168.1.1–192.168.1.254`.

**Зачем нужна?**

- Для разделения сетей и управления трафиком.
- Уменьшает широковещательный трафик.
