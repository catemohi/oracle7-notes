centos7-notes
=============

Заметки по работе с centOS 7.
-----------------------------

Добавление юзера
----------------

Для создания нового пользователя введите команду:

	sudo adduser username
	
Установить пароль для нового пользователя:

	sudo passwd username

Настройка DNS
-------------

в файле /etc/resolv.conf прописываем:

	nameserver 8.8.8.8

после перезапускаем network:

	systemctl restart network

Первоначальная настройка
------------------------

Первое, что необходимо сделать это поставить обновления:

	sudo yum update  sudo yum upgrade

После обновления для удобства рекомендую поставить Midnight Commander:

	sudo yum install mc -y

Устанавливаем сетевые утилиты:

	sudo yum install net-tools -y
	sudo yum install bind-utils -y
	sudo yum install wget git -y

Установка библиотек для работы Python и SSL:

	sudo yum -y groupinstall "Development tools"
	sudo yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel zlib* libffi-devel readline-devel tk-devel
	sudo yum -y install readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel

Устанавливаем vim:

	sudo yum install vim -y
	
Обертки для работы с zip:

	sudo yum install -y unzip zip

Отключаем SElinux:

	sudo setenforce 0

или отключаем перманентно:
	
	vim /etc/sysconfig/selinux
	SElinux=disable

USER IS NOT IN THE SUDOERS FILE
-------------------------------

В большинстве случаев в файле sudoers настроено так, что утилиту могут использовать все пользователи из группы wheel или sudo. Поэтому достаточно добавить нашего пользователя в эту группу. Для этого используйте команду usermod.

	usermod -a -G wheel имя_пользователя

Или:

	usermod -a -G sudo имя_пользователя

Вы также можете добавить нужную настройку для самого пользователя в файл sudoers, для этого добавьте в конец файла такую строку:

	имя_пользователя ALL = (ALL) ALL

Установка Python из исходников
------------------------------

Устанавливаем зависимость для сборки файлов из исходников:

	sudo yum groupinstall -y "Development Tools"

Скачиваем исходный файл Python

	wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tar.xz

Разархивируем и переходим в папку:

	tar -xJf Python-3.6.8.tar.xz
	cd Python-3.6.8

Запускаем конфигурационный скрипт:

	./configure

Компилируем с помощью make:

	make

Запускам установку:

	make install

Ошибки:

* zipimport.ZipImportError: can't decompress data; zlib not available

	Решение:

		yum install zlib-devel

	[Ссылка на проблему](https://unix.stackexchange.com/questions/291737/zipimport-zipimporterror-cant-decompress-data-zlib-not-available)


* SSL module is not available

	Решение:
	
		sudo yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel zlib* libffi-devel readline-devel tk-devel
		# переустановка python
		make install

Zsh и Oh-my-zsh
---------------

Установка:

  	yum install zsh -y
  	chsh -s /usr/bin/zsh
  	wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
  	/bin/cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
  	source ~/.zshrc
	
При ошибки изменения можно попробовать:
	
	usermod -s /bin/zsh 
	# вместо
	chsh -s /usr/bin/zsh


Обновление GCC:
---------------

Скачиваем исходник:

	wget https://ftp.gnu.org/gnu/gcc/gcc-8.2.0/gcc-8.2.0.tar.xz
	
Разархивируем:

	tar -xJf gcc-8.2.0.tar.xz
	
Проваливаемся в папку и скачиваем зависимости:

	./contrib/download_prerequisites
	
Запускаем конфигурацию:
	
	./configure --enable-multilib 

Если вылетает ошибка с zlib и GCC_NO_EXECUTABLES запустите добавив ключ:

	./configure --disable-multilib  --with-system-zlib
	# Так же есть решение не передавать ключ --disable-multilib, но так не запустилось
	Ошибка сборки компилятора с фатальной ошибкой: gnu/stubs-32.h: Нет такого файла или Каталог
	
error: gnu/stubs-32.h: No such file or directory

	Это сообщение об ошибке появляется в 64-битных системах, где GCC/UPC функция multilib включена, и это указывает на то, что 32-битная версия libc не установлен. 	Есть два способа исправить эту проблему:

	Установите 32-разрядную версию glibc (например, glibc-devel.i686 на Fedora, CentOS,..)
	Отключить сборку "multilib", предоставив "--disable-multilib" включить команду конфигурации компилятора

Запускаем make:

	sudo make && make install


