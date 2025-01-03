# 24 Пользователи и группы. Авторизация и аутентификация
## Описание домашнего задания
1. Запретить всем пользователям кроме группы admin логин в выходные (суббота и воскресенье), без учета праздников

```
vagrant ssh
sudo -i
```
```
    2  useradd otusadm && useradd otus
    3  passwd otusdm
    4  passwd otusadm
    5  passwd otus
    6  groupadd -f admin
    7  usermod otusadm -a -G admin && usermod root -a -G admin && usermod vagrant -a -G admin
```
![изображение](https://github.com/user-attachments/assets/34b7b439-be84-4c8f-a234-3f2059cff6b7)
![изображение](https://github.com/user-attachments/assets/1f2e7a54-3499-4a82-9f00-ec61e617e6ca)
```
root@pam:~# nano /usr/local/bin/login.sh
root@pam:~# cat /usr/local/bin/login.sh
#!/bin/bash
#Первое условие: если день недели суббота или воскресенье
if [ $(date +%a) = "Sat" ] || [ $(date +%a) = "Sun" ]; then
 #Второе условие: входит ли пользователь в группу admin
 if getent group admin | grep -qw "$PAM_USER"; then
        #Если пользователь входит в группу admin, то он может подключиться
        exit 0
      else
        #Иначе ошибка (не сможет подключиться)
        exit 1
    fi
  #Если день не выходной, то подключиться может любой пользователь
  else
    exit 0
fi
root@pam:~#  chmod +x /usr/local/bin/login.sh
root@pam:~# nano /etc/pam.d/sshd
root@pam:~# exit
```
![изображение](https://github.com/user-attachments/assets/46e83827-6d44-4a79-8d2d-7df07d6e8028)
![изображение](https://github.com/user-attachments/assets/ad0e1b98-427d-4a43-99f6-cd9473a673e3)


