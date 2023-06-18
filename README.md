# vpn
# Создаем Vagrantfile следующего содержания
![Vagrant_file](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/147838e0-920b-4a3a-a55e-208462e7500f)
# Создаем виртуальные машины используя команду vagrant up
![Create_vm](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/dc3bb592-0744-4c05-9895-4322c6229be8)
# Осуществляем подключение к серверу и клиенту 
( подключение к клиенту)
![conect_to_client](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/bed97f72-4f15-44a8-88c3-e776428a93fb) 
#( подключение к серверу)

![conetc_to_srv](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/e18099ad-fe9d-45cd-92d5-5b7240dfba99)  
# Устанавливаем репозиторий ( используем программу terminator что бы работать одновременно с двумя машинами)
![install_repo](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/0c4e209a-fcc8-410f-9901-92de90b77828)
# Устанавливаем пакеты openvpn и iperf3 на клиенте и на сервере
![install_opvpn](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/10d54daa-e692-46fd-a4e9-c92d39ea0d70)
# Отключаем SElinux
![Disable_SE](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/8d5e075c-2dd5-4134-b86e-d7cd9c6e9759)

# Приступим к найстройке  Openvpn на сервере:
- При помощи команды  openvpn --genkey --secret  создаем файл-ключ
![create_key_on_srv](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/99a5c5d2-a563-4916-a480-09d1da4be07c)
- Создаем конфигурационный файл сервера используя команду vi /etc/openvpn/server.conf
  ![server_conf](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/13f5ea2f-8e6d-4b2a-816d-5b5cb4b07ca4)
- serv.conf будем иметь следуюшее содержание
- ![serv](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/c2e2a9fe-419b-4e0f-8906-68e074b298d8)
# Создадим service unit  для запуска openvpn. Для этого используем команду vi /etc/systemd/system/openvpn@.service.
![create_unit](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/21b3ca64-4c21-488b-a0bd-d197cc064e99)
-Содержание service unit
![unit](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/2094bbf5-a33b-4c74-b1ae-3ab5ca187826)
#Осуществим запуск скрипта и добавим его в автозагрузку.
- systemctl start запускаем сервис
- systemctl enable добавляем в автозагрузку
- ![systemctl](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/d6c748fe-a1a8-448b-b82d-786a54649ffb)
# Настраиваем openvpn на клиенте:
-Создаем конфигурационный файл клиента server.conf
![client_vpn](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/5c75aaec-dd24-4d4a-a8bd-4dd3889ff8bd)
![client_conf](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/6af9a5d8-a56b-4dc3-95de-11737dae40b8)
На сервер клиента в директорию /etc/openvpn необходимо скопировать файл-ключ static.key, который был создан на сервере.
![image](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/8b4b9bce-2109-435e-ba8f-4301ad021d8c)
Замеряем скорость в туннеле.
● на openvpn сервере запускаем iperf3 в режиме сервера
iperf3 -s &
![image](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/06b61700-4534-4d87-bcb7-6db0154a8f52)

● на openvpn клиенте запускаем iperf3 в режиме клиента и замеряем
скорость в туннеле
iperf3 -c 10.10.10.1 -t 40 -i 5
![image](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/8d2f3be7-6f02-4cc5-9ac3-aaf7261c0005)

Проверяем режим  работы  tun. Для этого изменим конфигурационные файлы  дерективе dev

![image](https://github.com/AlexanderSerg-jun/vpn/assets/85576634/cf0d5b80-4336-4873-9e52-f8ecfd23052c)

# Как мы видим при использовании режима tun скорость гораздо выше. tun и tap используют разные уровни сети. Tun использует 3 уровень, tap -2 уровень. При использовании tap передаются пакеты с макадресами, чего не происходит при tun.

#2.RAS на базе OpenVPN
























