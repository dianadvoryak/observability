• http://localhost:5601 — Kibana (веб-интерфейс для просмотра логов, поиска и дашбордов).  
• http://localhost:9200 — Elasticsearch (REST API базы данных; при открытии в браузере отдает JSON со статусом кластера).  
• localhost:5044 — Logstash (Beats input) (TCP-порт для приема логов от агентов вроде Filebeat).  
• localhost:5000 — Logstash (TCP/JSON input) (TCP-порт для прямой отправки JSON-логов из приложений).  


## Локальный запуск ELK-стека (Elasticsearch, Logstash, Kibana)

Проект использует ELK-стек версии `8.17.0` в Docker Compose для сбора, хранения и визуализации логов.

### Запуск и остановка
1. Запустить контейнеры в фоновом режиме:
   ```
   docker compose up -d
   ```
2. Остановить контейнеры:
   ```
   docker compose down
   ```

### Доступные сервисы
* **Kibana (UI):** http://localhost:5601
* **Elasticsearch (API):** http://localhost:9200
* **Logstash (Beats TCP):** `localhost:5044`
* **Logstash (JSON TCP):** `localhost:5000`

### Проверка отправки логов


```
curl -X POST "http://localhost:9200/logs-test/_doc" -H "Content-Type: application/json" -d '{"message": "Hello Kibana", "@timestamp": "2026-11-19T13:42:00Z"}'
```

```
echo '{"message": "Тестовый лог", "level": "info"}' | nc localhost 5000
```

Главное отличие в том, в какой сервис вы отправляете данные: первая команда отправляет лог в Logstash (посреднику для обработки), а вторая — напрямую в Elasticsearch (в саму базу данных).  
• В реальном проекте ваше приложение должно слать логи по схеме первой команды (в Logstash на порт 5000 или 5044), чтобы не нагружать базу сырыми запросами и централизованно управлять форматом логов.

1. Откройте страницу Data Views:
	• Нажмите ≡ (три полоски в левом верхнем углу).
	• Прокрутите меню в самый низ до блока Management и нажмите Stack Management.
	• В левом меню настроек в блоке Kibana выберите Data Views.
	• (Или просто перейдите по ссылке: http://localhost:5601/app/management/kibana/dataViews)
2. Создайте шаблон:
	• Нажмите синюю кнопку Create data view в центре экрана.
	• В поле Name введите любое название (например, App Logs).
	• В поле Index pattern введите logs-* (справа сразу отобразится найденный индекс).
	• В выпадающем списке Timestamp field выберите @timestamp.
	• Нажмите Save data view to Kibana.
3. Посмотрите логи:
	• Перейдите в меню ≡ → Analytics → Discover — ваше тестовое сообщение сразу появится в таблице.



   
• Просмотр логов (Discover): http://localhost:5601/app/discover  
• Создание шаблона индекса (Data Views): http://localhost:5601/app/management/kibana/dataViews  
• Список всех индексов в базе: http://localhost:5601/app/management/data/index_management/indices  


