# Домашнее задание к занятию «ELK» - Савельев Алексей SYS-25
---
## Дополнительные ресурсы

При выполнении задания используйте дополнительные ресурсы:
- [docker-compose elasticsearch + kibana](11-03/docker-compose.yaml);
- [поднимаем elk в docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html);
- [поднимаем elk в docker с filebeat и docker-логами](https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html);
- [конфигурируем logstash](https://www.elastic.co/guide/en/logstash/7.17/configuration.html);
- [плагины filter для logstash](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html);
- [конфигурируем filebeat](https://www.elastic.co/guide/en/beats/libbeat/5.3/config-file-format.html);
- [привязываем индексы из elastic в kibana](https://www.elastic.co/guide/en/kibana/7.17/index-patterns.html);
- [как просматривать логи в kibana](https://www.elastic.co/guide/en/kibana/current/discover.html);
- [решение ошибки increase vm.max_map_count elasticsearch](https://stackoverflow.com/questions/42889241/how-to-increase-vm-max-map-count).
- [настройка filebeat VK.com](https://cloud.vk.com/docs/additionals/cases/cases-logs/case-logging#ustanovka_filebeat)
**Примечание**: если у вас недоступны официальные образы, можете найти альтернативные варианты в DockerHub, например, [такой](https://hub.docker.com/layers/bitnami/elasticsearch/7.17.13/images/sha256-8084adf6fa1cf24368337d7f62292081db721f4f05dcb01561a7c7e66806cc41?context=explore).

### Задание 1. Elasticsearch 

Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный. 

*Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name*.

---
### Ответ 1
---
- Что я делал:
1. Я воспользовался презентацией и устанавливал по данной схеме:
```bash
sudo apt update && sudo apt install gnupg apt-transport-https #<--зависимости
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key
add - #--добавляем gpg-ключ
echo "deb [trusted=yes] https://mirror.yandex.ru/mirrors/elastic/7/ stable
main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list #--добавляем репозиторий в apt
sudo apt update && sudo apt-get install elasticsearch #--устанавливаем elastic
sudo systemctl daemon-reload #--обновляем конфиги systemd
sudo systemctl enable elasticsearch.service #--включаем юнит
sudo systemctl start elasticsearch.service #--запускаем сервис
```
2. Зашёл в настройки `elasticsearch` по пути `/etc/elasticsearch/elasticsearch.yml`, раскомментировал строку `claster.name` и изменил в ней данные, написав своё имя.
3. Перезапустил сервис `sudo systemctl restart elasticsearch`
4. Проверяю изменённое имя кластера:
![cluster_health](https://github.com/Lexacbr/ELK/blob/main/scrsh/health-cmd.png)

---
### Задание 2. Kibana

Установите и запустите Kibana.

*Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty*.
```bash```
---
### Ответ 2
---
Что я делал:
1. Установка Кибаны:
```bash 
sudo apt install kibana
```
2. Обновляем конфигурационный файл systemd:
```bash
sudo systemctl daemon-reload
```
3. Включаю в автозагрузку сервис:
```bash
sudo systemctl enable kibana.service
```
4. Запускаю сервис:
```bash
sudo systemctl start kibana.service
```
5. Раскомментировал в конфиг-файле Кибаны `server.host:"0.0.0.0"`открыв доступ в Интернет
```bash
sudo nano /etc/kibana/kibana.yml
```
```bash
server.host:"0.0.0.0"
```
6. Перезагружаю сервис для применения настроек:
```bash
sudo systemctl restart kibana
```
7. На странице http://127.0.0.1:5601/app/dev_tools#/console, в консоли выполняю запрос GET /_cluster/health?pretty
```bash
GET /_cluster/health?pretty
```
![cluster_health](https://github.com/Lexacbr/ELK/blob/main/scrsh/clus-health.png)

---
### Задание 3. Logstash

Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.*

---
### Ответ 3
---




---
### Задание 4. Filebeat. 

Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.*

---
### Ответ 4
---



---
## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*. Доставка данных 

Настройте поставку лога в Elasticsearch через Logstash и Filebeat любого другого сервиса , но не Nginx. 
Для этого лог должен писаться на файловую систему, Logstash должен корректно его распарсить и разложить на поля. 

*Приведите скриншот интерфейса Kibana, на котором будет виден этот лог и напишите лог какого приложения отправляется.*

---
### Ответ 5
---



---