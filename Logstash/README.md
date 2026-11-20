
запрос:
```
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{"message": "Привет, Logstash!", "level": "info"}'
```

Проверьте логи контейнера:
```
docker logs -f logstash_solo
```
