# centos75
1. Подключил репозиторий #git clone https://github.com/dmitry-lyutenko/manual_kernel_update.git
2. Запустил vagrant #vagrant up
3. Подключился к виртуальной машине #vagrant ssh kernel-update
4. В ней подключил репозиторий ядра 5й версии, #sudo yum install -y http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
5. Установил его #sudo yum --enablerepo elrepo-kernel install kernel-ml -y
6. Обновил загрузчик #sudo grub2-mkconfig -o /boot/grub2/grub.cfg
7. И выбрал новое ядро для загрузки #sudo grub2-set-default 0
8. Перезагрузил #sudo reboot
9. После перезагрузки отобразилась новая версия ядра #uname -r
10. Далее на хостовой системе запустил создание образа пакером #packer build centos.json
-Согласно файла config.json был скачан образ http://mirror.yandex.ru/centos/7.8.2003/isos/x86_64/CentOS-7-x86_64-Minimal-2003.iso
-образ автоматически установлен на вм
-обновлено ядро (скрипт stage-1-kernel-update.sh) (elrepo-release-7.0-3.el7.elrepo.noarch.rpm)
-второй скрипт stage-2-clean.sh почистил логи, кэш и временные файлы, затем упаковал систему в образ
-выгружен образ вм в текущую директорию - это секция post-processors в фале centos.json
11. Для проверки образа импортировал его в vagrant #vagrant box add centos-7-5 centos-7-5 centos-7.7.1908-kernel-5-x86_64-Minimal.box
12. И в новой дирекории создал vagrantfile #vagrant init centos-7-5
13. В файле vagrantfile заменил строку :box_name => "centos-7-5-test",
13. Запустил вм #vagrant up
14. Подключился к ней #vagrant ssh centos-7-5-test
15. И проверил версию ядра #uname -r
5.6.11-1.el7.elrepo.x86_64

16. Полученный образ системы загрузил в Vagrant Cloud #vagrant cloud auth login
17. Ввёл логин и пароль от лк vagrantup.com
18. И опубликовал box #vagrant cloud publish --release Sergey-SSA/centos-7-5 1.0 virtualbox \
        centos-7.7.1908-kernel-5-x86_64-Minimal.box
ссылка на образ вм https://app.vagrantup.com/Sergey-SSA/boxes/centos-7-5
