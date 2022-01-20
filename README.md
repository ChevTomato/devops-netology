# Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

**1 Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.**

	$ type cd
	cd is a shell builtin - команда является втсроенной в оболочку
	Команды делаются встроенными либо из соображений производительности - встроенные команды исполняются быстрее, чем внешние, которые, как правило, запускаются в дочернем 	процессе, либо из-за необходимости прямого доступа к внутренним структурам командного интерпретатора. 

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
	$ ssh -t localhost 'tty' Принудительное выделение псевдотерминалов. Это может быть использовано для выполнения произвольных экранных программ на 
удаленной машине.

**13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, 
так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.**
	
	$ sudo reptyr -T PID (Перенос процесса в новый терминал)

**14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.**
	
	Основное использование команды tee – вывести стандартный вывод ( stdout) программы и записать его в файл.
	sudo не выполняет перенаправление вывода
	Tree получит вывод команды echo, повысит права на sudo и запишет в файл








# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

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





# devops-netology
 hello netology!

`В будущем, благодаря добавленному .gitignore, будут проигнорированны файлы:`

	состояния tfstate
	журнала сбоев crash.log
	содержащие конфедециальные данные .tfvars
	override?! override.tf override.tf.json *_override.tf *_override.tf.json
	tfplan?! *tfplan*
	конфигурации командной строки: .terraformrc terraform.rc
	New line
