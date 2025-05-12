Устанавливаю новую виртуалку и гостевые дополнения

![image](https://github.com/user-attachments/assets/ce591b6d-6d29-4c2f-95f3-da65aa0f637d)

Устанавливаю wget curl и git

`sudo yum install wget curl git`

![image](https://github.com/user-attachments/assets/6662cc44-218d-4957-be54-30b5c66e8d47)

Проверяю состояние фаерволла

`sudo firewall-cmd --state`

![image](https://github.com/user-attachments/assets/b6a3abc4-e0d3-48d0-a466-86a1a09b75e1)

Устанавливаю tar

`sudo yum install tar`

![image](https://github.com/user-attachments/assets/2184fe21-10e2-428f-9c06-5eb8118a1042)

Устанавливаю пакет chrony

`sudo yum install chrony`

![image](https://github.com/user-attachments/assets/d536ef0b-b37a-4388-9002-6b205a2187ef)

Включаю автозапуск chrony и запускаю службу

`systemctl enable chronyd`
`systemctl start chronyd`

![image](https://github.com/user-attachments/assets/0586e03d-680d-44fc-9c3f-15548e7b68aa)

Проверяю текущий режим SELinux

`getenforce`

![image](https://github.com/user-attachments/assets/2c761b02-7e02-4bdb-9405-7c05eef33b62)

Получаю ответ, что включена активная защита. Дальше перевожу SELinux в разрешающий режим. А также постоянно отключаю SELinux через конф файл

`sudo setenforce 0`
`sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config`

![image](https://github.com/user-attachments/assets/28803733-6589-4276-8cfa-a51b9ae5b88a)

Скачиваю прометиус

`wget https://github.com/prometheus/prometheus/releases/download/v3.3.0/prometheus-3.3.0.linux-amd64.tar.gz`

![image](https://github.com/user-attachments/assets/e6781f70-ca3d-44ec-a004-afbfe08244b9)

Создаю директории для прометиуса

`sudo mkdir -p /etc/prometheus /var/lib/prometheus`

![image](https://github.com/user-attachments/assets/e7f772b7-dbbc-4378-a1fb-291ce4916cca)

Распаковываю архив, который я скачал

`tar -zxf prometheus-*.linux-amd64.tar.gz`

![image](https://github.com/user-attachments/assets/6141c267-e696-4ff2-9398-60506f01e2cb)

Перехожу в папку, проверяю корректность её с помощью `pwd`. А дальше копирую файлы прометиуса в системные директории:

`sudo cp prometheus promtool /usr/local/bin/`
`sudo cp prometheus.yml /etc/prometheus/`

![image](https://github.com/user-attachments/assets/c8e6f37f-55e7-42dc-bbce-b9bdfc30dac9)

Очищаю временные файлы и проверяю дикторию с помощью pwd

`cd .. && rm -rf prometheus-*.linux-amd64/ && rm -f prometheus-*.linux-amd64.tar.gz`

![image](https://github.com/user-attachments/assets/e94058a1-f836-44be-83b5-45a65d13673c)

Содержимое директории с помощью

`ls -l`

![image](https://github.com/user-attachments/assets/058f4191-3d6f-40af-858f-81187092e584)



