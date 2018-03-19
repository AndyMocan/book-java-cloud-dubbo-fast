# RabbitMQ

---

## docker-compose.yml

```
version: "3.1"
services:
  rabbitmq-management:
    image: rabbitmq:management
    restart: always
    container_name: rabbitmq
    environment:
      - RABBITMQ_DEFAULT_USER=lusifer
      - RABBITMQ_DEFAULT_PASS=123456
      - RABBITMQ_DEFAULT_VHOST=DEFAULT_HOST
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - /usr/local/docker/rabbitmq/data:/var/lib/rabbitmq
```