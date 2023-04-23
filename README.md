# system_monitoring
Monitoring dashboard for PC

## Install

1. Обновить версии образов в `docker-compose.yml` при необходимости
2. Изменить пути для volumes в `docker-compose.yml` при необходимости
3. Запустить контейнеры командой `docker-compose up --build -d`
4. Зайти в Grafana: http://localhost:3000/ (admin/admin) и задать новый пароль
5. Проверить доступность Prometheus: http://localhost:9090
6. В Grafana настроить новый источник данных: 
   1. Settings -> Data Sources -> Add Data Source -> выбрать Prometheus
   2. В URL вставить http://prometheus:9090 -> Save & test
7. Импортировать дашборд из `dashboards/` или выбрать любой 
из https://grafana.com/grafana/dashboards/: Dashboards/[Manage]/Import
8. Проверить доступность Node Exporter: http://localhost:9100/metrics
9. Настроить сбор метрик для Prometheus из Node Exporter
   1. Узнать container_id для Prometheus: `docker ps`
   2. `docker exec -it <container_id> /bin/sh`
   3. `vi etc/prometheus/prometheus.yml`
   4. Добавить в конец по аналогии строки
      1. Нажать `i` для вставки текста
      2. ```
           - job_name: "node-exporter"                                                                    
             static_configs:                                                                              
             - targets: ["node-exporter:9100"]
         ```
      3. Нажать `ESC` для режима ввода команд
      4. Сохранить и выйти `:wq`
   5. `exit`
   6. Перезапустить контейнер с Prometheus `docker restart <container_id>`
   7. Перейти в Prometheus и проверить State UP для Node Exporter в Status -> Targets
10. Вернуться в Grafana и в Dashboards сверху на панели выбрать Job: node-exporter
   