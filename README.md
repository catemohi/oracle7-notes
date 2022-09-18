centos7-notes
=============

Заметки по работе с centOS 7.
-----------------------------

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
	yum install wget git -y

Установка библиотек для работы Python и SSL:

	sudo yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel zlib* libffi-devel readline-devel tk-devel

Устанавливаем vim:

	sudo yum install vim -y

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

	![Ссылка на проблему](https://unix.stackexchange.com/questions/291737/zipimport-zipimporterror-cant-decompress-data-zlib-not-available)


* SSL module is not available

	Решение:
		
		sudo yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel zlib* libffi-devel readline-devel tk-devel
		переустановка python make install

Zsh и Oh-my-zsh
---------------

  yum install zsh -y
  chsh -s /usr/bin/zsh
  wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
  /bin/cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
  source ~/.zshrc

