Начал я с того, что поставил Oracle Linux с гостевыми дополнениями на виртуалку. Образ скачал с официального сайта, взял LTS версию (чтоб всё стабильно работало и не приходилось часто обновляться). Настроил виртуалку на 2 процессора и 4 ГБ оперативки — вроде хватает для базовых задач. 😎

Первым делом решил поставить Wget — штука полезная, чтобы качать файлы прямо из терминала. Удобно, когда нужно что-то быстро скачать без лишних телодвижений.

`sudo yum install wget`

![image](https://github.com/user-attachments/assets/ef61a945-3287-4ee3-aa48-94c418fb0c5e)

Дальше поставил Curl — это такая программа, которая позволяет работать с кучей протоколов (HTTP, HTTPS, FTP и т.д.). Очень удобно для тестирования API или просто скачивания данных.

`sudo yum install curl`

![image](https://github.com/user-attachments/assets/6d697f85-57ad-49eb-843f-6ae5a95c886a)

Чтобы поставить Docker, нужно было добавить его официальный репозиторий. Я скачал его для CentOS (Oracle Linux на нём основан, так что подошло).

`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

![image](https://github.com/user-attachments/assets/e2347def-9502-40df-a41c-cc4fd0e1e566)


Теперь самое интересное — установка Docker. Поставил основные компоненты: docker-ce, docker-ce-cli и containerd.io.

`sudo yum install docker-ce docker-ce-cli containerd.io`

![image](https://github.com/user-attachments/assets/648e5b26-46b1-417d-84f1-beabaee19e1c)

Скрин подтверждения установки.

![image](https://github.com/user-attachments/assets/c2e5c377-efd8-46a2-8e7c-5787480c3c43)

Чтобы Docker запускался сам при старте системы, выполнил команду:

`sudo systemctl enable docker --now`

![image](https://github.com/user-attachments/assets/124d6187-7412-4bcb-bc84-f0418f5a2bb3)

Теперь Docker всегда готов к работе! 😊

Установка Docker Compose 🛠️

Для работы с несколькими контейнерами нужен Docker Compose. Сначала я определил последнюю версию через API GitHub:

`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

![image](https://github.com/user-attachments/assets/f256970e-f5c5-4676-94a7-1523d006bf24)

Затем скачал и установил Docker Compose:

`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/2ede5873-9bec-4188-9957-614a0bbb3b57)


Настройка прав и проверка версии Docker Compose ✅

Дал права на выполнение файлу Docker Compose и проверил его версию:

`sudo chmod +x /usr/bin/docker-compose`
`docker-compose --version`

![image](https://github.com/user-attachments/assets/82f9588f-212c-42b4-9b84-370413fe4733)

Проверил права на файл:

`ls -l /usr/bin/docker-compose`

и файл стал исполняемым:

![image](https://github.com/user-attachments/assets/7d1db63a-96c4-4c36-8d9e-2837c4f63ba0)

Права -rwxr-xr-x означают, что файл можно читать, писать и выполнять. Всё ок! 👍

Установка Grafana через Git 📊

Для мониторинга ставлю Grafana. Сначала установил Git, так как его не было в системе:

![image](https://github.com/user-attachments/assets/94eea3b0-67e7-46da-a888-c30b59182c88)

Затем клонировал репозиторий и настроил структуру каталогов:

`cd grafana_stack_for_docker`
Сначала я перешёл в папку grafana_stack_for_docker, где лежат все необходимые файлы для настройки Grafana.

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

Чтобы всё было организовано, я создал необходимые папки для Grafana и её компонентов. Команда mkdir -p создаёт все промежуточные каталоги, если они ещё не существуют.

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`

Теперь у меня есть папки для конфигурации Grafana, её данных и данных Prometheus. 📁

Чтобы не запускать всё от root, я изменил владельца созданных папок на текущего пользователя. Это делается с помощью команды chown.

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

Для Grafana нужен конфигурационный файл grafana.ini. Если его нет, команда touch создаст пустой файл. Если файл уже существует, она просто обновит его временные метки.
`touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

`cp config/* /mnt/common_volume/swarm/grafana/config/`
Все файлы из папки config я скопировал в созданную директорию для Grafana.

`mv grafana.yaml docker-compose.yaml` 
Файл grafana.yaml нужно переименовать в docker-compose.yaml, чтобы Docker Compose мог его использовать.

`sudo docker compose up -d`

Наконец, я запустил контейнеры в фоновом режиме с помощью Docker Compose.

Скрин со всеми командами

![image](https://github.com/user-attachments/assets/119098bf-5747-4736-a7fd-894536d8033e)

и работа докера композа

![image](https://github.com/user-attachments/assets/f2cc0f46-d72f-44eb-b148-6fffe6f11042)

результат

![image](https://github.com/user-attachments/assets/0ae262c5-5e60-46aa-8c51-ebfe0dd45e13)

Команда `sudo docker compose up -d` позволяет запустить контейнеры, описанные в файле docker-compose.yml, в фоновом режиме (демонизированном режиме). Ключ -d (от англ. detached) обеспечивает запуск контейнеров без блокировки терминала, что позволяет продолжать работу в командной строке. Это особенно полезно при работе с долго работающими сервисами, такими как веб-серверы или базы данных.

`sudo docker compose up -d`

![image](https://github.com/user-attachments/assets/24673996-0732-4972-bb19-ea4a7840255c)

Команда `sudo docker compose stop` останавливает все активные контейнеры, связанные с проектом, который описан в файле docker-compose.yml. Остановка контейнеров не удаляет их, а лишь приостанавливает их выполнение. Это полезно, когда нужно временно остановить сервисы, например, для технического обслуживания или обновления конфигурации.

`sudo docker compose stop`

![image](https://github.com/user-attachments/assets/458355d2-e2dc-4049-a59a-672fd5664522)

Команда `sudo docker compose` down выполняет следующие действия:

Останавливает все активные контейнеры, связанные с проектом.

Удаляет остановленные контейнеры.

Удаляет сети, созданные автоматически для этого проекта.

По умолчанию команда не удаляет тома (volumes) и образы (images), если они явно не указаны в параметрах. Это полезно для полной очистки проекта, например, при завершении работы или пересоздании окружения.

`sudo docker compose down`

![image](https://github.com/user-attachments/assets/ff5f6f74-5145-4514-be2d-baf77e7a59fc)

Команда `sudo docker compose ps` выводит список контейнеров, которые относятся к текущему проекту, описанному в docker-compose.yml. В выводе отображается состояние каждого контейнера (работает, остановлен и т.д.), а также дополнительная информация, такая как порты и имена контейнеров. Это полезно для мониторинга состояния сервисов.

`sudo docker compose ps`

![image](https://github.com/user-attachments/assets/223897b0-3257-4a4b-b833-30cd0f179311)

`git clone https://github.com/ROMABLUNT/grafana_college.git`

Команда `git clone https://github.com/ROMABLUNT/grafana_college.git` клонирует удалённый репозиторий с GitHub на локальную машину. В результате создаётся папка с именем grafana_college, содержащая все файлы и историю изменений из репозитория. Это стандартный способ начать работу с проектом, размещённым на GitHub.

![image](https://github.com/user-attachments/assets/b6088044-75f3-46f9-bfa6-8b49dedf306f)

`pwd`

Команда `pwd `(от англ. print working directory) выводит полный путь к текущей рабочей директории. Это полезно, когда нужно узнать, в какой папке вы находитесь в терминале, особенно при работе с множеством директорий или при выполнении сложных скриптов.

![image](https://github.com/user-attachments/assets/93155505-c60b-4566-afa7-ca13a53f4a1c)

Конфигурирую файл на node exporter, добавляю данные с гитхаба разработчика

![image](https://github.com/user-attachments/assets/3d9d16f3-8c55-42fc-9c30-3ed4b1654c2f)
![image](https://github.com/user-attachments/assets/c56a0ada-cd68-408a-acdf-fb14f75ced8b)

Также конфигурирую файл prometheus, добавляю exporter мой

![image](https://github.com/user-attachments/assets/6d17ff63-6b92-494d-a6ed-b2a6f47c3053)


![image](https://github.com/user-attachments/assets/79de452c-6524-4d9b-bf29-e8a09d19aa61)


Создаю dashboard 

![image](https://github.com/user-attachments/assets/78fd1a36-01c6-4e60-9744-40a85a7bd753)

Создаю визуализацию

![image](https://github.com/user-attachments/assets/e7ae39aa-be01-4262-9d70-d8ebf048a2e5)
![image](https://github.com/user-attachments/assets/480e7eab-5b66-49d8-b366-08c1d7dbca1c)

Выбираю прометиус

![image](https://github.com/user-attachments/assets/2a57a1e5-5d3d-49a8-bbfd-50577552ce63)


Ставлю URL прометиуса

![image](https://github.com/user-attachments/assets/b4d3a027-49c3-493e-b378-0e097e558474)

Аутентификацию устанавливаю. admin admin

![image](https://github.com/user-attachments/assets/e84c4bdd-b77a-473a-ac1c-8f3541502a85)

Сохраняю и проверяю, что всё хорошо

![image](https://github.com/user-attachments/assets/40c72839-35e2-4bb8-b74d-6126519d5d33)


Результат

![image](https://github.com/user-attachments/assets/2d3abf42-691a-4247-a519-f1a8b121c121)


VICTORIA METRICS

В докер компоуз уже изначально введено про victoria metrics и при запуске она работает.

я создаю копию моего прометиуса и меняю название на вика


![image](https://github.com/user-attachments/assets/d9398785-93eb-4a07-add6-4be84504c67e)

ввожу команды

![image](https://github.com/user-attachments/assets/abf27fb4-6301-44f3-bb66-378b7598f0f7)


и в итоге в графане

![image](https://github.com/user-attachments/assets/669df77b-8d1d-469d-8023-fe1a9282dd94)

и в виктории

![image](https://github.com/user-attachments/assets/f20fe0b1-d153-4c6e-b125-e0a8f9d3cdea)








