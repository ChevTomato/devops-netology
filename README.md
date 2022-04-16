## Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"

<details>


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
???
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
???
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

### Ваш скрипт:
```python
???
```

### Пример работы скрипта:
???
</details>








## Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"
<details>
	
## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Ошибка, при попытки сложить число и строку  |
| Как получить для переменной `c` значение 12?  | c = str(a) + str(b)  |
| Как получить для переменной `c` значение 3?  | c = int(a) + int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
import os

bash_command_path = ["cd ~/sysadm-homeworks", "pwd"]
path = os.popen(' && '.join(bash_command_path)).read().rstrip() + '/'

bash_command = ["cd ~/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
$ python3 2.py
/home/vagrant/sysadm-homeworks/README.md
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
import os
import sys

if len(sys.argv) < 2:
    sys.exit()
    
bash_command_path = [f"cd {sys.argv[1]}", "pwd"]
path = os.popen(' && '.join(bash_command_path)).read().rstrip() + '/'

bash_command = [f"cd {sys.argv[1]}", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
$ python3 3.py ~/sysadm-homeworks/
/home/vagrant/sysadm-homeworks/README.md
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
from socket import gethostbyname

file = open("test_file", "r")
lines = file.readlines()
dns = {line.split()[0]: line.split()[1] for line in lines}

for name in dns.keys():
    if gethostbyname(name) != dns[name]:
        print(f"[ERROR] {name} IP mismatch: {dns[name]} {gethostbyname(name)}")

    print(f"{name}: {gethostbyname(name)}")
```

### Вывод скрипта при запуске при тестировании:
```
$ python3 4.py
[ERROR] drive.google.com IP mismatch: 0.0.0.0 173.194.73.194
drive.google.com: 173.194.73.194
[ERROR] mail.google.com IP mismatch: 0.0.0.0 173.194.73.19
mail.google.com: 173.194.73.19
[ERROR] google.com IP mismatch: 0.0.0.0 74.125.205.101
google.com: 74.125.205.101
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так получилось, что мы очень часто вносим правки в конфигурацию своей системы прямо на сервере. Но так как вся наша команда разработки держит файлы конфигурации в github и пользуется gitflow, то нам приходится каждый раз переносить архив с нашими изменениями с сервера на наш локальный компьютер, формировать новую ветку, коммитить в неё изменения, создавать pull request (PR) и только после выполнения Merge мы наконец можем официально подтвердить, что новая конфигурация применена. Мы хотим максимально автоматизировать всю цепочку действий. Для этого нам нужно написать скрипт, который будет в директории с локальным репозиторием обращаться по API к github, создавать PR для вливания текущей выбранной ветки в master с сообщением, которое мы вписываем в первый параметр при обращении к py-файлу (сообщение не может быть пустым). При желании, можно добавить к указанному функционалу создание новой ветки, commit и push в неё изменений конфигурации. С директорией локального репозитория можно делать всё, что угодно. Также, принимаем во внимание, что Merge Conflict у нас отсутствуют и их точно не будет при push, как в свою ветку, так и при слиянии в master. Важно получить конечный результат с созданным PR, в котором применяются наши изменения. 

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```
</details>








## Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

<details>

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `c`  | a+b  | строковое значение, так как отстутствуют $ |
| `d`  | 1+2  | строковое значение, обращение к переменным $ |
| `e`  | 3  | число, конструкция (( )) подразумевает арифмитические действия |


## Обязательная задача 2
	
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

### Ваш скрипт:
```bash

#!/usr/bin/env bash

while ((1==1))
do
  curl https://localhost:4757
  if (($? != 0))
  then
    date >> curl.log
  else
    exit 0
  fi
  sleep 1
done

```

## Обязательная задача 3
Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
#!/usr/bin/env bash

run_test(){
 for ((i=1; i < 6; i++)); do
  curl http://$1:80

  if [ $? == "0" ]; then
   result="active"
  else
   result="failed"
  fi

  date_time="$(date)"
  echo $date_time $1 $result >> curl.log
 done
}

run_test "192.168.0.1"
run_test "173.194.222.113"
run_test "87.250.250.242"
```

## Обязательная задача 4
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
#!/usr/bin/env bash

run_test(){
 while :; do
  curl http://$1:80 > /dev/null #2>&1

  if (( $? != 0 )); then
   date_time="$(date)"
   echo $date_time $1 Fail >> error.log
   exit 1
  fi

  sleep 2
 done
}

run_test "192.168.0.1"
run_test "173.194.222.113"
run_test "87.250.250.242"
```	

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```	
	
</details>	














## Домашнее задание к занятию "3.9. Элементы безопасности ИС"

<details>

<br>**1 Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей**<br>
`Установил, зарегистрировался, синхронизировал`

**2 Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP**<br>
`Установил, настроил`
	
**3 Установите apache2, сгенерируйте самоподписанный сертификат, настройте сайт для работы по HTTPS**<br>
	
 Устанавливаем `$ sudo apt update` `$ sudo apt install apache2`<br>
 Разрешаем `$ sudo ufw allow "Apache Full"`<br>
 Подключаем SSL mod	`$ sudo a2enmod ssl`<br>
 Рестартим	`$ sudo systemctl restart apache2`<br><br>
 Создаём SSL сертификат:
```	
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=Netology/OU=Org/CN=192.168.0.17"
```
<br>
	
Настраиваем Apache для использования SSL: `$ sudo vim /etc/apache2/sites-available/192.168.0.17.conf`
<br>	
```
<VirtualHost *:443>   
	ServerName 192.168.0.17   
	DocumentRoot /var/www/192.168.0.17   
	SSLEngine on   
	SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt   
	SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```
Создаём корневую деррикторию нашего сайта `$ sudo mkdir /var/www/192.168.0.17`<br>
Создаём в корне наш index.html `$ sudo nano /var/www/192.168.0.17/index.html`	
```
<h1>it worked!</h1>
```


Включаем наш новый сайт в Апаче `$ sudo a2ensite 192.168.0.17`<br>
Тестируем на ошибки синтаксиса `$ sudo apache2ctl configtest`<br>
Рестартим `$ sudo systemctl reload apache2`

![dz-apacha](https://user-images.githubusercontent.com/95047357/155121572-0964790b-0209-4850-aa25-a8577b9d19a9.PNG)
	
	
<br><br>
	
**4 Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК и тому подобное...)**

`$ git clone --depth 1 https://github.com/drwetter/testssl.sh.git` `$ ./testssl.sh -U --sneaky https://www.netology.ru`

	SWEET32  VULNERABLE, uses 64 bit block ciphers
	TLS1: 	 VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
	LUCKY13  potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches


<br><br>**5 Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу**
	
Устанавливаем `$ sudo apt install openssh-server`<br>
Проверяем `$ sudo systemctl status ssh`<br>
Генерируем ключ на клиенте `$ ssh-keygen`<br>
Копируем его на сервер `$ ssh-copy-id -i ~/.ssh/id_rsa  chev@192.168.0.103`<br>
Подключаемся `ssh chev@192.168.0.103`<br>

<br><br>**6 Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера**

`$ mv .ssh/id_rsa .ssh/vagrant_rsa` `$ mv .ssh/id_rsa.pub .ssh/vagrant_rsa.pub` `$ vim .ssh/config`:
```	    
Host server
	HostName 192.168.0.103
	User chev
	Port 22
	IdentityFile ~/.ssh/vagrant_rsa
```	
Подключаемся `$ ssh server`

<br><br>**7 Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark**

	$ tcpdump -D
	$ sudo tcpdump -w dump.pcap -c 100 -i eth0
	
```
Опции утилиты tcpdump
-i Задает интерфейс, с которого необходимо анализировать трафик (без указания интерфейса - анализ "первого попавшегося").
-n Отключает преобразование IP в доменные имена. nn - запрещается преобразование номеров портов в название протокола.
-e Включает вывод данных канального уровня (например, MAC-адреса).
-v Вывод дополнительной информации (TTL, опции IP).
-s Указание размера захватываемых пакетов. (по-умолчанию - пакеты больше 68 байт)
-w Задать имя файла, в который сохранять собранную информацию.
-r Чтение дампа из заданного файла.
-p Захватывать только трафик, предназначенный данному узлу. (захват всех пакетов, например в том числе широковещательных).
-q Переводит tcpdump в "бесшумный режим", пакет анализируется на транспортном уровне (TCP, UDP, ICMP), а не на сетевом (IP).
-t Отключает вывод меток времени.
```
	
![shark](https://user-images.githubusercontent.com/95047357/155134063-fddfbd53-403e-4832-8d44-4012b41cea43.JPG)

	
<br><br>**8 Просканируйте хост scanme.nmap.org. Какие сервисы запущены?**

	$ nmap scanme.nmap.org
```
PORT      STATE SERVICE
22/tcp    open  ssh		Secure Shell
80/tcp    open  http		Hypertext Transfer Protocol
9929/tcp  open  nping-echo	Network packet generation tool / ping utility
31337/tcp open  Elite 		Cult of the Dead Cow Protocol 
```
<br><br>**9 Установите и настройте фаервол ufw на web-сервер из задания 3. Откройте доступ снаружи только к портам 22,80,443**

Узнаём статус файрвола `$ sudo ufw status`<br>
Запускаем `$ sudo ufw enable`<br>
Запрещаем входящие `sudo ufw default deny incoming`<br>
Разрешаем исходящие `sudo ufw default allow outgoing`<br>
Добавляем сервисы из /etc/services `sudo ufw allow ssh` `sudo ufw allow 80` `sudo ufw allow https`<br>
Проверяем `$ sudo ufw status verbose`
```	
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip
To                         Action      From
80,443/tcp (Apache Full)   ALLOW IN    Anywhere
22                         ALLOW IN    Anywhere
```

<br>
</details>














## Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

<details>
	
<br>**1 Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP**
```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```
```
> show ip route 213.171.59.22
Routing entry for 213.171.32.0/19, supernet
... ok
```
```
> show bgp 213.171.59.22
BGP routing table entry for 213.171.32.0/19, version 886094
... ok
```
	
<br>**2 Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации**	
* Создаём dummy0 интерфейс	
```	
$ modprobe -v dummy
$ lsmod | grep dummy
dummy                  16384  0
$ ip link add dummy0 type dummy
$ ip addr add 10.0.0.2/24 dev dummy0
$ ip link set dummy0 up
$ ip a
```
```	
1: lo: inet 127.0.0.1/8 scope host lo
2: eth0: inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
3: dummy0: <BROADCAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether 36:29:f2:70:c0:11 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.2/24 scope global dummy0
       valid_lft forever preferred_lft forever
    inet 10.0.0.3/24 scope global secondary dummy0
       valid_lft forever preferred_lft forever
    inet6 fe80::3429:f2ff:fe70:c011/64 scope link 
       valid_lft forever preferred_lft forever
```
* Добавляем маршрут
```
$ ip route add 192.192.0.0/15 dev dummy0
$ ip route
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.0.0/24 dev dummy0 proto kernel scope link src 10.0.0.2
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
192.192.0.0/15 dev dummy0 scope link	
```	
	
<br>**3 Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров**
```	
$ ss -tpan
State      Recv-Q     Send-Q         Local Address:Port         Peer Address:Port     Process     
LISTEN     0          4096           127.0.0.53%lo:53                0.0.0.0:*                    
LISTEN     0          128                  0.0.0.0:22                0.0.0.0:*                    
ESTAB      0          0                  10.0.2.15:22               10.0.2.2:45332                
LISTEN     0          128                     [::]:22                   [::]:*     	
```
* 53 - DNS, 22 - SSH <br>
	
<br>**4 Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?**		
```
$ ss -upan
State      Recv-Q     Send-Q         Local Address:Port         Peer Address:Port     Process          
UNCONN     0          0              10.0.2.15%eth0:68          0.0.0.0:*  
UNCONN     0   	
``` 
* 68 - Bootstrap Protocol <br>	
	
<br>**5 Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали**	
	
![HomeNet](https://user-images.githubusercontent.com/95047357/154648628-66bbca8f-b334-41e6-8e34-0c19236c0d89.png)
	
	
<br>
</details>
	
	
	
	
	
	
	
	
	
	
	

## Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

<details>
	
<br>**1 Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?**

Linux: `ip a` `ip link show` `ifconfig -a`
	
Win: `ipconfig /all`
	
<br>**2 Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?**

`LLDP` — протокол канального (L2) уровня, который позволяет сетевым устройствам анонсировать в сеть информацию о себе и о своих возможностях, а также собирать эту информацию о соседних устройствах. 

Установка `apt install lldpd && systemctl enable lldpd && systemctl start lldpd`
Пакет `lldpd`
Команда `lldpcli sh int` информацию по интерфейсам привязанным к lldp, а `lldpctl` покажет соседей: 
```b
# lldpctl
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
```
	
<br>**3 Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.**

Технология VLAN позволяет произвести сегментацию локальной сети на подсети по функциональному признаку независимо от территориального расположения устройств. <br>Установка `apt install vlan` Пакет в Ubuntu `vlan`. Команда `vconfig` и `ip route`

Пример конфига с Vlan ID-100 для интерфейса eth0 (/etc/network/interfaces):
```a 
auto eth0.100
iface eth0.100 inet static
address 192.168.1.200
netmask 255.255.255.0
vlan-raw-device eth0
```
	
<br>**4 Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.**

Типы агрегаций в интерфейсе Linux
	
`mode0 balance-rr` Последовательно кидает пакеты, с первого по последний интерфейс.<br>
`mode1 active-backup` Один из интерфейсов активен. Если активный интерфейс выходит из строя (link down и т.д.), другой интерфейс заменяет активный. Не требует дополнительной настройки коммутатора<br>
`mode2 balance-xor` Передачи распределяются между интерфейсами на основе формулы ((MAC-адрес источника) XOR (MAC-адрес получателя)) % число интерфейсов. Один и тот же интерфейс работает с определённым получателем. Режим даёт балансировку нагрузки и отказоустойчивость.<br>
`mode3 broadcast` Все пакеты на все интерфейсы<br>
`mode4 802.3ad` Link Agregation — IEEE 802.3ad, требует от коммутатора настройки.<br>
`mode5 balance-tlb` Входящие пакеты принимаются только активным сетевым интерфейсом, исходящий распределяется в зависимости от текущей загрузки каждого интерфейса. Не требует настройки коммутатора.<br>
`mode6 balance-alb` Тоже самое что 5, только входящий трафик тоже распределяется между интерфейсами. Не требует настройки коммутатора, но интерфейсы должны уметь изменять MAC.<br>
* `active-backup` и `broadcast` обеспечивают только отказоустойчивость<br>
`balance-tlb`, `balance-alb`, `balance-rr`, `balance-xor` и `802.3ad` обеспечат отказоустойчивость и балансировку
<br><br>
* `active-backup`, `balance-tlb` и `balance-alb` работают "сами по себе", можно настроить только на одном хосте<br>
`broadcast`, `balance-rr`, `balance-xor` и `802.3ad` потребуют настройки ещё и коммутатора.	
<br><br>
* active-backup <br> Пример настройки интерфейсов eth0 и eth1 /etc/network/interfaces:	
```a
auto bond0
iface bond0 inet dhcp
   bond-slaves none
   bond-mode active-backup
   bond-miimon 100

auto eth0
   iface eth0 inet manual
   bond-master bond0
   bond-primary eth0 eth1

auto eth1
iface eth1 inet manual
   bond-master bond0
   bond-primary eth0 eth1
```
* balance-alb <br> (bonds - поясняем за bonding, bond0 - имя интерфейса, interfaces - объеденяемые интерфейсы, mode - мод bonding, mii-monitor-interval - интервал мониторинга 2 сек.):
```a
bonds:
    bond0:
      dhcp4: yes
      interfaces: [eth0, eth1]
      parameters: 
        mode: balance-alb
        mii-monitor-interval: 2
```
	
<br>**5 Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.**
```
$ ipcalc -b 10.10.10.0/29

Address:   10.10.10.0
Netmask:   255.255.255.248 = 29
Wildcard:  0.0.0.7
Network:   10.10.10.0/29
HostMin:   10.10.10.1
HostMax:   10.10.10.6
Broadcast: 10.10.10.7
Hosts/Net: 6                     Class A, Private Internet
Итого: 8 адресов = 6 для хостов, 1 адрес сети и 1 широковещательный адрес.

Битовая маска: /29
Сетевая маска: 255.255.255.248
Адрес сети: 10.10.10.0/29
Первый хост: 192.168.1.1
Последний хост: 192.168.1.6
Широковещательный адрес: 192.168.1.7

Сеть с маской /24 можно разбить на 32 подсети с маской /29
	
Примеры /29 подсетей:
10.10.10.0/29
10.10.10.8/29
10.10.10.16/29
10.10.10.32/29 
итд
```

<br>**6 Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.**

Можно взять адреса из сети для CGNAT - 100.64.0.0/10.
```
$ ipcalc -b 100.64.0.0/10 -s 50
HostMin:   100.64.0.1
HostMax:   100.127.255.254
Hosts/Net: 4194302               	
		
1. Requested size: 50 hosts
Netmask:   255.255.255.192 = 26 
Network:   100.64.0.0/26        
HostMin:   100.64.0.1           
HostMax:   100.64.0.62          
Broadcast: 100.64.0.63          
Hosts/Net: 62  

Маска для диапазона из 62 хостов /26
```


<br>**7 Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?**

* Проверить таблицу: Linux `ip neigh`, `arp -n` Windows: `arp -a`
	
* Очистить кеш: Linux `ip -s neigh flush all` Windows: `arp -d *`

* Удалить один IP: Linux `ip neigh delete <IP> dev <INTERFACE>`, `arp -d <IP>` Windows: `arp -d <IP>`

<br>
</details>









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

```a
$ dig -x 8.8.8.8 | grep PTR
8.8.8.8.in-addr.arpa.           IN      PTR
8.8.8.8.in-addr.arpa.   219     IN      PTR     dns.google.
```


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
	vagrant ssh-config > vagrant-ssh
	 
 **5 Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные 
 ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?**
 
	RAM:1024mb, CPU:2, HDD:64gb, video:4mb

**6 Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора 
 виртуальной машине?**
 
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
	config.vm.provider "virtualbox" do |v|
	v.memory = 1024
  	v.cpus = 2
	end
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




## Devops-Netology
 
<details>

благодаря добавленному .gitignore, будут проигнорированны файлы:

	состояния tfstate
	журнала сбоев crash.log
	содержащие конфедециальные данные .tfvars
	override?! override.tf override.tf.json *_override.tf *_override.tf.json
	tfplan?! *tfplan*
	конфигурации командной строки: .terraformrc terraform.rc
	New line<br><br>
</details>
