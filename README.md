
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

## Решение Задача 1

```bash
docker build -t killakazzak/custom-nginx:1.0.0 .
```

![image](https://github.com/user-attachments/assets/8d049413-670b-4209-b69c-ec66b3df33c4)

```bash
docker push  killakazzak/custom-nginx:1.0.0
```
![image](https://github.com/user-attachments/assets/7e1fc314-40c8-424a-aa54-abf4f034add7)

https://hub.docker.com/repository/docker/killakazzak/custom-nginx/general

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Решение Задача 2

 docker run -d --name  "tenda-nginx-t2" -p 127.0.0.1:8080:80 killakazzak/custom-nginx:1.0.0

![image](https://github.com/user-attachments/assets/bc6b0230-148c-4395-a989-195670c6dd65)

```bash
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs tenda-nginx-t2 -n1 ; docker exec -it tenda-nginx-t2 base64 /usr/share/nginx/html/index.html
```

![image](https://github.com/user-attachments/assets/b455ec79-d072-46a5-a864-f08b3e57a32f)



## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Решение Задача 3

```bash
docker attach tenda-nginx-t2
```
Ctrl-C

![image](https://github.com/user-attachments/assets/46365b13-5456-4e36-9363-1176126935b0)

```bash
docker ps -a
```
![image](https://github.com/user-attachments/assets/02f59d39-8e10-450a-a2fc-d2b028cd0624)

Контейнер остановился так как комбинация Ctrl-C, отправляет сигнал прерывания (SIGINT) контейнеру. Это приводит к остановке процесса, который выполняется в контейнере.

```bash
docker start tenda-nginx-t2
```
![image](https://github.com/user-attachments/assets/0304324a-4845-45d2-ade6-32edb5b552e0)

```bash
docker ps -a
```
![image](https://github.com/user-attachments/assets/d9a0d9c4-78ec-46ff-a716-81d7ebf70e44)

```bash
docker exec -it tenda-nginx-t2 bash
```
![image](https://github.com/user-attachments/assets/93b81250-67cf-4c73-9ea7-29cb651d1192)

```bash
apt-get update
apt-get install nano
```
![image](https://github.com/user-attachments/assets/889f13e2-a9cf-4df2-ac2d-0c5c21d72e01)


```bash
nano /etc/nginx/conf.d/default.conf
```
![image](https://github.com/user-attachments/assets/fb653359-e7fa-4b89-b873-c2330403d8d3)

```bash
nginx -s reload
```
![image](https://github.com/user-attachments/assets/941e27a2-1a63-44ab-b107-b6116a913c84)

```bash
curl http://127.0.0.1:80
curl http://127.0.0.1:81
```
![image](https://github.com/user-attachments/assets/b3a8475b-283a-43c9-bffc-3bbad5abfa6d)

```Ctrl+D```
![image](https://github.com/user-attachments/assets/9f752577-5b1d-46cd-8939-c90cbb22a21f)

```bash
ss -tlpn | grep 127.0.0.1:8080``` , ```docker port tenda-nginx-t2```, ```curl http://127.0.0.1:8080
```
![image](https://github.com/user-attachments/assets/6889a3d1-22ce-4570-9056-6049c08e7191)

Суть проблемы заключается в том, что изменили порт, на котором Nginx слушает входящие соединения, с 80 на 81, также порт 8080 не будет перенаправляется порт 81 поэтому запросы на 8080 не будут работать.

```bash
docker stop tenda-nginx-t2
systemctl stop docker.socket
vim /var/lib/docker/containers/cad288a4bb8870a431c04af4523288de6b41505d6694239ffe7070199a297182/hostconfig.json
```
![image](https://github.com/user-attachments/assets/e350c90f-bec3-407e-9549-325d756882db)

```bash
vim /var/lib/docker/containers/cad288a4bb8870a431c04af4523288de6b41505d6694239ffe7070199a297182/config.v2.json
```
![image](https://github.com/user-attachments/assets/6153efdc-f48f-4ea9-9cd1-dba8146153dc)

```bash
systemctl start docker
docker start tenda-nginx-t2
docker ps
```
![image](https://github.com/user-attachments/assets/9b03089b-946a-460f-b99c-66685de9fde4)

```bash
curl http://127.0.0.1:8080
```
![image](https://github.com/user-attachments/assets/c927127e-2e76-4064-978f-a3cab20bf7b1)

```bash
docker rm -f tenda-nginx-t2
docker ps -a
```
![image](https://github.com/user-attachments/assets/f6119aa0-e1b0-412f-9d63-4dbee0041d91)



## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Решение Задача 4

```bash
docker run -d --name centos-container -v $(pwd):/data centos tail -f /dev/null
docker run -d --name debian-container -v $(pwd):/data debian tail -f /dev/null
```

![image](https://github.com/user-attachments/assets/8b7ba506-5ec0-4f09-aca7-b745f1295dfe)

```bash
docker exec -it centos-container bash
echo "test" > /data/centos-file.txt
```

```Ctrl+D```

```bash
echo "test" > host-file.txt
docker exec -it debian-container bash
ls -l /data/
```
![image](https://github.com/user-attachments/assets/92acb993-0488-4e4b-806e-ed2df817e0d2)


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.



## Решение Задача 5

```bash
mkdir -p /tmp/netology/docker/task5
```
compose.yaml
```
cat > /tmp/netology/docker/task5/compose.yaml <<EOL
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
EOL
```
docker-compose.yaml

```
cat > /tmp/netology/docker/task5/docker-compose.yaml <<EOL
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
      - "5000:5000"
EOL
```
```
docker compose up -d
```
![image](https://github.com/user-attachments/assets/f9965670-a598-46fc-9fef-60b8f98e1e7b)

При выполнении команды docker compose up -d будет запущен только файл ```docker-compose.yaml```, если не указать, какой файл использовать. Docker Compose по умолчанию ищет файл ```docker-compose.yaml``` в текущем каталоге. Если необходимо запустить оба файла, вам нужно будет изменить ```docker-compose.yaml```, чтобы включить содержимое ```compose.yaml```.

```bash
cat > /tmp/netology/docker/task5/docker-compose.yaml <<EOL
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

  registry:
    image: registry:2
    network_mode: host
    ports:
      - "5000:5000"
EOL
```

```bash
docker compose up -d
docker ps -a
```
![image](https://github.com/user-attachments/assets/3b0775cf-8b7e-4a39-8a20-3d1d7584e6e7)

```bash
docker tag killakazzak/custom-nginx:1.0.0 custom-nginx:latest
docker images
```
![image](https://github.com/user-attachments/assets/50390099-6f61-458b-9d48-0cd93512480c)



![image](https://github.com/user-attachments/assets/ff0205ae-7e81-4542-b5d9-f65a1d01b500)

![image](https://github.com/user-attachments/assets/645f6191-239c-416d-b6a0-e39ead8190e5)

![image](https://github.com/user-attachments/assets/edc5f832-18f1-4e85-8b53-11b9e56096d4)

![image](https://github.com/user-attachments/assets/e130f387-52c4-49f1-9b70-4dcbd50e9e57)


---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


