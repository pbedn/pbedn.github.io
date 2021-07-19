+++
title = "Guide: MySQL and Python (WSL2) + Pycharm (Windows)"
date = "2020-09-30"
draft = false
tags = ['ubuntu', 'wsl', 'python', 'mysql']
+++

Let's connect MySQL database installed in Ubuntu (WSL2) with Pycharm Professional on Windows. 

<!--more-->

#### Windows: WSL + Ubuntu

WSL (Windows Subsystem for Linux) has two versions. The latest (version 2) is much improved and recommended,
the only thing to remember is to have Windows version at least 2004 (check with ```winver``` command in the console). 
Microsoft has good guides [how to set up WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
I am using Ubuntu 20.04, which has Python 3.8 by default.

#### Ubuntu: MySQL + Python environment

Steps in order that MySQL could be accessed in Windows on 127.0.0.1:3307

* Install MySQL database ([step-by-step-guide](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database))

* Set the default authentication method (for PyCharm to be able to connect)
```
$ sudo mysql -u root -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';
mysql> exit
```

* Add configuration to '/etc/mysql/my.cnf' so database can be seen:
```
[mysqld]
port=3307 # choose free db port
bind-address=0.0.0.0
```

* Restart database
```
sudo service mysql restart
``` 

* Install Python virtual environment in Home/.virtualenvs directory
```
cd && mkdir .virtualenvs && cd .virtualenvs
sudo apt install python3-venv
python3 -m venv .venv
```

#### Windows: PyCharm

* Add new Python interpreter -> WSL, with settings
```
Linux Distribution: Ubuntu 20.04
Python interpreter path: /home/<your user>/.virtualenvs/.venv/bin/python
```

* Install missing Python packaging tools (it can be done from PyCharm directly with linux sudo password)
* Connect to MySQL database at localhost:3307 (I had to change server timezone to my local - Europe/Warsaw)

And that is it! Our application can be developed using an Ubuntu environment but Windows tools.
I have git set up on Windows as well, and that is why I keep files here. But it is possible to store files in 'Ubuntu directory'.
From Windows, we can easily map a network drive to Ubuntu using path ```\\wsl$\Ubuntu-20.04\```, and add it to Pycharm Project structure.


#### Summary

Windows as a machine for the developer has changed over the years, from 'evil monster corporate tool' to 'powerful and elegant'.
Some years ago my setup option was to either dual boot or have Linux inside VirtualBox. Now it is much easier. 
And I do not have to restart the computer to fight another Cylon in Battlestar Galactica strategy game.. ;)
