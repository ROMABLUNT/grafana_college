Установлен oracle linux с гостевыми дополнениями на виртуальную машину. Скачал образ с официального сайта LTS версия. Закинул пару процессоров, пару гигов оперативной памяти.

Для начала скачиваю Wget. Консольная программа для загрузки файлов по сети. Результат: Успешно, без ошибок.

`sudo yum install wget`

![image](https://github.com/user-attachments/assets/ef61a945-3287-4ee3-aa48-94c418fb0c5e)

Устанавливаю curl. Cлужебная программа командной строки, позволяющая взаимодействовать с множеством различных серверов по множеству различных протоколов с синтаксисом URL

`sudo yum install curl`

![image](https://github.com/user-attachments/assets/6d697f85-57ad-49eb-843f-6ae5a95c886a)

Скачиваю репозиторий docker.

`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

![image](https://github.com/user-attachments/assets/e2347def-9502-40df-a41c-cc4fd0e1e566)


Устанавливаю docker. 

`sudo yum install docker-ce docker-ce-cli containerd.io`

![image](https://github.com/user-attachments/assets/648e5b26-46b1-417d-84f1-beabaee19e1c)

Скрин подтверждения установки.

![image](https://github.com/user-attachments/assets/c2e5c377-efd8-46a2-8e7c-5787480c3c43)

Запускаю docker и разрешаю автозапуск

`sudo systemctl enable docker --now`

![image](https://github.com/user-attachments/assets/124d6187-7412-4bcb-bc84-f0418f5a2bb3)

Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose

`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

![image](https://github.com/user-attachments/assets/f256970e-f5c5-4676-94a7-1523d006bf24)

Загружаю и устанавливаю последнюю версию docker

`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/2ede5873-9bec-4188-9957-614a0bbb3b57)


Предоставление прав на выполнение файла docker-compose и просмотр версии докера

`sudo chmod +x /usr/bin/docker-compose`
`docker-compose --version`

![image](https://github.com/user-attachments/assets/82f9588f-212c-42b4-9b84-370413fe4733)

После делаю проверку с помощью

`ls -l /usr/bin/docker-compose`

и файл стал исполняемым:

![image](https://github.com/user-attachments/assets/7d1db63a-96c4-4c36-8d9e-2837c4f63ba0)

-rwxr-xr-x

указывает, что файл имеет права на чтение, запись и исполнение для владельца, и права на чтение и исполнение для группы и других пользователей.

Ставлю графану через гит, параллельно гит и устанавливая. Так как его изначально нет.

![image](https://github.com/user-attachments/assets/94eea3b0-67e7-46da-a888-c30b59182c88)

`cd grafana_stack_for_docker`
• переход в папку

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

• команда создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги, если они ещё не существуют.

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`
• команда создаёт структуру каталогов для Grafana и связанных с ней компонентов, если они ещё не существуют.

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`
• все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе

`touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

• файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

`cp config/* /mnt/common_volume/swarm/grafana/config/`
• команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

`mv grafana.yaml docker-compose.yaml` 
• команда переименовывает файл grafana.yaml в docker-compose.yaml. Ничего не покажет, но можно проверить при помощи команды ls

`sudo docker compose up -d`

• команда создает и запускает контейнеры в фоновом режиме, используя конфигурацию из файла docker-compose.yml, с правами суперпользователя.

Скрин со всеми командами
![image](https://github.com/user-attachments/assets/119098bf-5747-4736-a7fd-894536d8033e)




