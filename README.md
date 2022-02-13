## Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

<details>
	
	
**1 Работа c HTTP через телнет.**
	
- Подключитесь утилитой телнет к сайту stackoverflow.com, `telnet stackoverflow.com 80` <br>В ответе укажите полученный HTTP код, что он означает? Отправьте HTTP запрос:
	```bash
	GET /questions HTTP/1.0
	HOST: stackoverflow.com
	```
	```
	HTTP/1.1 301 Moved Permanently
	cache-control: no-cache, no-store, must-revalidate
	location: https://stackoverflow.com/questions
	x-request-guid: 54dd7a02-b0cd-47e4-be9a-1b5cd472c87e
	feature-policy: microphone 'none'; speaker 'none'
	content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
	Accept-Ranges: bytes
	Date: Thu, 10 Feb 2022 15:57:04 GMT
	Via: 1.1 varnish
	Connection: close
	X-Served-By: cache-hel1410034-HEL
	X-Cache: MISS
	X-Cache-Hits: 0
	X-Timer: S1644508624.212794,VS0,VE110
	Vary: Fastly-SSL
	X-DNS-Prefetch-Control: off
	Set-Cookie: prov=fbfd317a-d943-7508-c2f4-992b443cb564; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly
	Connection closed by foreign host.
	```
Код перенаправления " 301 Moved Permanently " протокола (HTTP) показывает, что запрошенный ресурс был окончательно перемещён в URL, указанный в заголовке Location https://stackoverflow.com/questions
	
	
**2. Повторите задание 1 в браузере, используя консоль разработчика F12.**
	
- откройте вкладку `Network`, отправьте запрос http://stackoverflow.com, найдите первый ответ HTTP сервера, откройте вкладку `Headers`, укажите в ответе полученный HTTP код. проверьте время загрузки страницы, какой запрос обрабатывался дольше всего? приложите скриншот консоли браузера в ответ.
	
	![image](https://user-images.githubusercontent.com/95047357/153454117-1ea500c5-587a-419a-8865-87bf210c7025.png)
	
	Status Code: 307 Internal Redirect
	Загрузка контента с основной страницы заняла 459ms
	
**3. Какой IP адрес у вас в интернете?**
	
	$ curl ifconfig.co
	46.39.229.16
	
**4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`**
	
	$ whois 46.39.229.16
	route: 46.39.228.0/23
	descr: GORCOM-NET
	origin: AS29124
	mnt-by: ISKRATELECOM-MNT
	
**5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`**
```ebru	
$ traceroute -AnI 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  10.0.2.2 [*]  0.807 ms  0.758 ms  0.747 ms
 2  192.168.0.7 [*]  7.795 ms  7.804 ms  7.992 ms
 3  10.4.224.1 [*]  8.093 ms  8.347 ms  8.453 ms
 4  95.143.212.112 [AS29124]  8.581 ms  8.658 ms  8.785 ms
 5  81.200.9.237 [AS29124]  9.357 ms *  9.339 ms
 6  81.200.9.150 [AS29124]  9.931 ms  5.871 ms  5.468 ms
 7  108.170.250.33 [AS15169]  6.290 ms  8.501 ms  8.808 ms
 8  108.170.250.51 [AS15169]  7.709 ms  7.765 ms  8.043 ms
 9  216.239.51.32 [AS15169]  25.754 ms * *
10  172.253.66.110 [AS15169]  26.356 ms  26.877 ms  27.520 ms
11  142.250.56.219 [AS15169]  24.425 ms  25.006 ms  20.066 ms
12  * * *
21  8.8.8.8 [AS15169]  23.021 ms *  23.134 ms
```
	
**6. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?**

Наибольшая задержка на участке `AS15169 108.170.250.51`
```ebr	
$ mtr 8.8.8.8 -znrc 1
Start: 2022-02-10T18:14:21+0000
HOST: vagrant                     Loss%   Snt   Last   Avg  Best  Wrst StDev
  1. AS???    10.0.2.2             0.0%     1    1.0   1.0   1.0   1.0   0.0
  2. AS???    192.168.0.7          0.0%     1    4.5   4.5   4.5   4.5   0.0
  3. AS???    10.4.224.1           0.0%     1    6.4   6.4   6.4   6.4   0.0
  4. AS29124  95.143.212.112       0.0%     1    5.5   5.5   5.5   5.5   0.0
  5. AS29124  81.200.9.237         0.0%     1    4.6   4.6   4.6   4.6   0.0
  6. AS29124  81.200.9.150         0.0%     1    3.9   3.9   3.9   3.9   0.0
  7. AS15169  108.170.250.33       0.0%     1    5.4   5.4   5.4   5.4   0.0
  8. AS15169  108.170.250.51       0.0%     1   33.2  33.2  33.2  33.2   0.0
  9. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 10. AS15169  172.253.66.110       0.0%     1   22.7  22.7  22.7  22.7   0.0
 11. AS15169  142.250.56.219       0.0%     1   25.0  25.0  25.0  25.0   0.0
 12. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 13. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 14. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 15. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 16. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 17. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 18. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 19. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 20. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 21. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 22. AS15169  8.8.8.8              0.0%     1   21.3  21.3  21.3  21.3   0.0
```
	
**7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой `dig`**
```ns	
$ dig +short NS dns.google
ns2.zdns.google.
ns1.zdns.google.
ns4.zdns.google.
ns3.zdns.google.
```
```a	
$ dig +short A dns.google
8.8.4.4
8.8.8.8
```
	
**8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой `dig`**



</details>







## Домашнее задание к занятию "3.5. Файловые системы"

<details>
	
**1 Узнайте о sparse (разряженных) файлах.**

	Файлы с пустотами на диске. Записи пустот на диск не происходит. Используются в образах VM, торрентах, резервных копиях дисков.

**2 Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?** 

	Не могут, так имеют одинаковый индексный дескриптор (inode), в котором хранятся права доступа и имя владельца

**3 Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:...**

	$ lsblk
	> Появились sdb и sdc    

**4 Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.**
                
	$ fdisk /dev/sdb

	Command (m for help): F
	Command (m for help): n
	Select (default p): p
	Partition number (1-4, default 1):
	First sector (2048-5242879, default 2048):
	Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G

	Created a new partition 1 of type 'Linux' and of size 2 GiB.
	Command (m for help): n
	Select (default p): p
	Partition number (2-4, default 2):
	First sector (4196352-5242879, default 4196352):
	Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):

	Created a new partition 2 of type 'Linux' and of size 511 MiB.
	Command (m for help): w
	The partition table has been altered.
	Calling ioctl() to re-read partition table.
	Syncing disks.

**5 Используя sfdisk, перенесите данную таблицу разделов на второй диск.**

	$ sfdisk -d /dev/sdb | sfdisk /dev/sdc

	sdb                         8:16   0  2.5G  0 disk
	├─sdb1                      8:17   0    2G  0 part
	└─sdb2                      8:18   0  511M  0 part
	sdc                         8:32   0  2.5G  0 disk
	├─sdc1                      8:33   0    2G  0 part
	└─sdc2                      8:34   0  511M  0 part

**6 Соберите mdadm RAID1 на паре разделов 2 Гб.**

	$ mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[bc]1

	sdb                         8:16   0  2.5G  0 disk
	├─sdb1                      8:17   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdb2                      8:18   0  511M  0 part
	sdc                         8:32   0  2.5G  0 disk
	├─sdc1                      8:33   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdc2                      8:34   0  511M  0 part

**7 Соберите mdadm RAID0 на второй паре маленьких разделов.**

	$ mdadm --create /dev/md1 --level=0 --raid-devices=2 /dev/sd[bc]2

	sdb                         8:16   0  2.5G  0 disk
	├─sdb1                      8:17   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdb2                      8:18   0  511M  0 part
	  └─md1                     9:1    0 1018M  0 raid0
	sdc                         8:32   0  2.5G  0 disk
	├─sdc1                      8:33   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdc2                      8:34   0  511M  0 part
	  └─md1                     9:1    0 1018M  0 raid0

**8 Создайте 2 независимых PV на получившихся md-устройствах.**

	$ pvcreate /dev/md1 /dev/md0
	
	$ pvs
	PV         VG        Fmt  Attr PSize    PFree
	/dev/md0             lvm2 ---    <2.00g   <2.00g
	/dev/md1             lvm2 ---  1018.00m 1018.00m
 	/dev/sda3  ubuntu-vg lvm2 a--   <63.00g  <31.50g

**9 Создайте общую volume-group на этих двух PV.**

	$ vgcreate vg0 /dev/md1 /dev/md0
	Volume group "vg0" successfully created

	$ vgs
	VG        #PV #LV #SN Attr   VSize   VFree
	ubuntu-vg   1   1   0 wz--n- <63.00g <31.50g
  	vg0         2   0   0 wz--n-  <2.99g  <2.99g

**10 Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.**

	$ lvcreate -L 100M vg0 /dev/md1
  	Logical volume "lvol0" created. 

**11 Создайте mkfs.ext4 ФС на получившемся LV.**

	$ mkfs.ext4 /dev/vg0/lvol0
	Done

**12 Смонтируйте этот раздел в любую директорию, например, /tmp/new.**

	$ mkdir /tmp/new
	$ mount /dev/vg0/lv0 /tmp/new/

**13 Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.**
	
	$ ls /tmp/new/
	lost+found  test.gz

**14 Прикрепите вывод lsblk.**

	$ lsblk

	NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                         8:0    0   64G  0 disk
	├─sda1                      8:1    0    1M  0 part
	├─sda2                      8:2    0    1G  0 part  /boot
	└─sda3                      8:3    0   63G  0 part
	  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
	sdb                         8:16   0  2.5G  0 disk
	├─sdb1                      8:17   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdb2                      8:18   0  511M  0 part
	  └─md1                     9:1    0 1018M  0 raid0
	    └─vg0-lvol0           253:1    0  100M  0 lvm   /tmp/new
	sdc                         8:32   0  2.5G  0 disk
	├─sdc1                      8:33   0    2G  0 part
	│ └─md0                     9:0    0    2G  0 raid1
	└─sdc2                      8:34   0  511M  0 part
	  └─md1                     9:1    0 1018M  0 raid0
	    └─vg0-lvol0           253:1    0  100M  0 lvm   /tmp/new

**15 Протестируйте целостность файла:**

	$ gzip -t /tmp/new/test.gz
	$ echo $?
	0

**16 Используя pvmove, переместите содержимое PV с RAID0 на RAID1.**

	$ pvmove /dev/md1 /dev/md0
	  /dev/md1: Moved: 4.00%
	  /dev/md1: Moved: 100.00%

**17 Сделайте --fail на устройство в вашем RAID1 md.**

	$ mdadm --fail /dev/md0 /dev/sdb1
	mdadm: set /dev/sdb1 faulty in /dev/md0

**18 Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.**

	$ dmesg | grep md0 | tail -n 2

	[36903.545333] md/raid1:md0: Disk failure on sdb1, disabling device.
        md/raid1:md0: Operation continuing on 1 devices.

**19 Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:**

	$ gzip -t /tmp/new/test.gz
	$ echo $?
	0

**20 Погасите тестовый хост, vagrant destroy**
	
	$ vagrant destroy

</details>










## Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

<details>
	
**1 На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
поместите его в автозагрузку, предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.**
	
Установка node_exporter
	
	$ wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
	$ tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz
Создание полльзователя службы
	
	$ sudo useradd -r -M -s /bin/false node_exporter
	
Создаем unit файл
	
	$ sudo vim /etc/systemd/system/node_exporter.service

	[Unit]
	Description=Prometheus Node Exporter
	Wants=network-online.target
	After=network-online.target

	[Service]
	User=node_exporter
	Group=node_exporter
	Type=simple
	ExecStart=/home/vagrant/node_exporter-1.3.1.linux-amd64/node_exporter $EXTRA_OPTS

	[Install]
	WantedBy=multi-user.target

Запуск процесса
	
	$ sudo systemctl daemon-reload
	$ sudo systemctl enable --now node_exporter.service

Разрешаем порт по умолчанию
	
	$ sudo iptables -A INPUT -p tcp --dport 9100 -j ACCEPT

Проверяем статус
	
	$ sudo systemctl status node_exporter.service
	Active: active (running)
	
**2 Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.**

Процессор 
	
	$ curl http://localhost:9100/metrics | grep node_cpu
	node_cpu_seconds_total{cpu="0",mode="user"} — время выполнения процессов, которые выполняются в режиме пользователя.
	node_cpu_seconds_total{cpu="0",mode="system"} — время выполнения процессов, которые выполняются в режиме ядра.

Память 
	
	$ curl http://localhost:9100/metrics | grep node_memory
	node_memory_MemTotal_bytes — общий объем памяти на машине.
	node_memory_MemFree_bytes — объем свободной памяти, которая может быть освобождена.
	
Диск 
	
	$ curl http://localhost:9100/metrics | grep node_disk
	node_disk_read_time_seconds_total — количество секунд, затраченных на чтение.
	node_disk_io_now — количество операций ввода-вывода (I/O), выполняемых в настоящий момент.
	
Сеть 
	
	$ curl http://localhost:9100/metrics | grep node_network
	node_network_receive_bytes_total — объем полученных данных (в байтах).
	node_network_receive_errs_total — количество возникших ошибок при получении.
	
**3 Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.**

	> vagrant port
	22 (guest) => 2222 (host)
	80 (guest) => 8080 (host)
 	19999 (guest) => 19999 (host)

	Chrome > localhost:19999 > Netdata Ok
	
**4 Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?**

Можно 
	
	$ dmesg | grep -i 'Hypervisor detected'
	[    0.000000] Hypervisor detected: KVM
	(Kernel-based Virtual Machine)	
	
**5 Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?**

	$ sysctl fs.nr_open
	1048576 - максимальное количество файловых дескрипторов, которое может выделить процесс (1024 * 1024 = 1048576).
	$ ulimit -n 
	1024 - мягкий лимит на пользователя (может быть изменен)

**6 Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.**
	
	> PTS/0
	$ sudo -i
	$ unshare -f --pid --mount-proc /bin/sleep 1h
	
	> PTS/1
 	$ sudo -i
	$ ps a | grep /bin/sleep
	3728 pts/0    S+     0:00 unshare -f --pid --mount-proc /bin/sleep 1h
   	3729 pts/0    S+     0:00 /bin/sleep 1h
   	3746 pts/1    S+     0:00 grep --color=auto /bin/sleep
	
	$ nsenter --target 3729 --pid --mount
	$ ps aux
	USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
	root           1  0.0  0.0   5476   592 pts/0    S+   06:56   0:00 /bin/sleep 1h
	root           2  0.1  0.4   7236  4060 pts/1    S    06:58   0:00 -bash
	root          13  0.0  0.3   8892  3408 pts/1    R+   06:58   0:00 ps aux

**7 Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?**

	Fork bomb, определяет функцию с именем : вызывает саму себя дважды в фоновом режиме, с последующим делением. 
	cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope
	Автоматическая стабилизация CGROUP, в виде ограничения на максимальное количество процессов пользователя с id 1000
	Чсло процессов пользователя меняется через ulimit -u (число)
	
</details>








## Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

<details>
	
**1 Какой системный вызов делает команда cd? Вам нужно найти тот единственный, который относится именно к cd.**
	
	$ strace /bin/bash -c 'cd /tmp' 2>&1 | grep tmp
	chdir("/tmp")

**2 Используя strace выясните, где находится база данных file на основании которой она делает свои догадки**
	
	$ strace file /dev/tty
	/usr/share/misc/magic.mgc

**3 Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению 
переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно 
заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на 
файловой системе).**
	
	$ truncate -s 0 /proc/PID/fd/3

**4 Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?**
	
	данные процессы не выполняются и ресурсы не потребляют

**5 На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04**
	
	$ sudo opensnoop-bpfcc
	PID    COMM               FD ERR PATH
	882    vminfo              4   0 /var/run/utmp
	690    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	690    dbus-daemon        20   0 /usr/share/dbus-1/system-services
	690    dbus-daemon        -1   2 /lib/dbus-1/system-services
	690    dbus-daemon        20   0 /var/lib/snapd/dbus-1/system-services/

**6 Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, 
где можно узнать версию ядра и релиз ОС.**
	
	$ strace uname -a
	вызов uname()
	$ sudo apt install manpages-dev
	$ man 2 uname
	Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.

**7 Чем отличается последовательность команд через ; и через && в bash? Есть ли смысл использовать в bash &&, если применить set -e?**
	
	&& условный оператор (логическое и), вывод hi только при наличии  /tmp/some_dir иначе завершится
	;  - разделитель последовательных команд, выполнит даже с ошибкой
	set -e и && не имеет смысла, так как в случае ошибки выполнение прекратиться.

**8 Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?**
	
	set-e прекращает выполнение команды если команда завершилась ошибкой
	set-u прекращает выполнение команды если встретилась несуществующая переменная
	set-x выводит выполняемые команды в stdout перед выполненинем
	set-o прекращает выполнение скрипта, даже если одна из частей пайпа завершилась ошибкой. Использование в сценариях упрощает отслеживание ошибок, создаёт более читаемый вывод и создаёт завершение при базовых ошибках.

**9 Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES)
что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).**
	
	$ ps -o stat
	STAT
	Ss 
	R+
	T Остановлен по сигналу контроля задачи или из-за отслеживания (трассировки)
	S Процессы ожидающие завершения, s лидер сесии
	R Запущен или запускаем (на очереди запуска), + Находится в группе процессов переднего плана
	
</details>


## Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

<details>

**1 Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.**

	$ type cd
	cd is a shell builtin - команда является втсроенной в оболочку
	Команды делаются встроенными либо из соображений производительности - встроенные команды исполняются быстрее, чем внешние, которые, как правило, запускаются в дочернем процессе, либо из-за необходимости прямого доступа к внутренним структурам командного интерпретатора. 

**2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.**

	$ grep will has_been_moved.txt
	will_be_moved
	$ grep will has_been_moved.txt -c
	1

**3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?**

	$ top, $ htop, $ pstree -p, $ pgrep systemd
	PID 1 = systemd

**4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?**

	$ ls -l nodir 2>/dev/pts/1
	ls: cannot access 'nodir': No such file or directory

**5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример**

	$ cat <has_been_moved.txt> out.txt
	$ cat out.txt
	will_be_moved

**6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?**

	получится $ echo hello >/dev/pts/1
	echo Hello from pts3 to tty3 >/dev/tty3

**7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?**

	Дескриптор 5 перенаправляется в stdout
	echo запишет значение netology в пятый дескриптор и выведет его

**8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по 
умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через 
промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.**

	$ ls /nodir 3>&2 2>&1 1>&3 |grep No -c
	1

**9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?**

	Выводятся переменные окружения. $ env $ printenv

**10. Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe**
	
	/proc/<PID>/cmdline Этот файл, доступный только для чтения, содержит полную командную строку для процесса
	/proc/<PID>/exe символическая ссылка содержащую фактический путь к выполняемой команде

**11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo**
	
	$ grep sse /proc/cpuinfo
	sse4_2

**12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в 
лекции 3.2. Однако:**
	
	$ man ssh
	$ ssh -t localhost 'tty' Принудительное выделение псевдотерминалов. Это может быть использовано для выполнения произвольных экранных программ на удаленной машине.

**13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, 
так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.**
	
	$ sudo reptyr -T PID (Перенос процесса в новый терминал)

**14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.**
	
	Основное использование команды tee – вывести стандартный вывод ( stdout) программы и записать его в файл.
	sudo не выполняет перенаправление вывода
	Tree получит вывод команды echo, повысит права на sudo и запишет в файл

</details>


## Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

 <details>
	
**1 Установите средство виртуализации Oracle VirtualBox.**

	Virtualbox 6.1

**2 Установите средство автоматизации Hashicorp Vagrant.**

	Vagrant 2.2.19

 **3 В вашем основном окружении подготовьте удобный для дальнейшей работы терминал.**

	WinTerm, MobaXterm (ssh -F vagrant-ssh default)

**4 С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:**

	vagrant init, up

 **5 Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные 
 ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?**
 
	RAM:1024mb, CPU:2, HDD:64gb, video:4mb

**6 Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора 
 виртуальной машине?**
 
	VagrantFile:
	config.vm.provider "virtualbox" do |v|
  	  v.memory = 1024
  	  v.cpus = 2
	end
	
 **7 Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных 
 настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.**
 
	ok!
	
 **8 Ознакомиться с разделами man bash, почитать о настройках самого bash:
 какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?**
 
	HISTFILESIZE - максимальное число строк в файле истории для сохранения, строка 1155(688)
	HISTSIZE - число команд для сохранения, строка 1178(699)
	
 **что делает директива ignoreboth в bash?**
 
	ignoreboth сокращение двух директив ignorespace and ignoredups, 
    	ignorespace - не сохранять команды начинающиеся с пробела, 
    	ignoredups - не сохранять команду, если такая уже имеется в истории
	
 **9 В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?**
 
	Зарезервированные слова, подстановка элементов из списока, строка 343(221)
	Например touch file_{a,b,c}.txt создаст file_a.txt,file_b.txt,file_c.txt
	
 **10 С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?**
 
	создание ста тысяч файлов touch {000001..100000}, проверка find . -maxdepth 1 -type f | wc
	максимум даёт создать 139600 файлов
	Максимальная длина аргумента для функций ограничена значением ARG_MAX (getconf ARG_MAX 2097152), 300000 не влезут.
	Увеличить объём пространства для стека с 8192КБ до 65536КБ можно командой ulimit -s 65536
	
 **11 В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]**
 
	проверяет условие -d /tmp и возвращает ее статус 0 или 1, наличие катаолга /tmp

 **12 Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе 
	type -a bash в виртуальной машине наличия первым пунктом в списке:**
	
	mkdir /tmp/new_path_dir/
	cp /bin/bash /tmp/new_path_dir/
	PATH=/tmp/new_path_dir/:$PATH
	type -a bash
	
 **13 Чем отличается планирование команд с помощью batch и at?**
 
	batch — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше 1.5
	at — используется для назначения одноразового задания на заданное время
	
 **14 Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.**
 
	vagrant suspend
</details>




## devops-netology
 hello netology!

благодаря добавленному .gitignore, будут проигнорированны файлы:

	состояния tfstate
	журнала сбоев crash.log
	содержащие конфедециальные данные .tfvars
	override?! override.tf override.tf.json *_override.tf *_override.tf.json
	tfplan?! *tfplan*
	конфигурации командной строки: .terraformrc terraform.rc
	New line
