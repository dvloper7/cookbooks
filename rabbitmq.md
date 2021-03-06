# RabbitMQ

## CLI

### Installation

#### Homebrew

```sh
brew install rabbitmq
```

### Service

```sh
# Homebrew
brew services start rabbitmq
```

## Docker

### Running

```sh
docker run -d \
  $(echo "$DOCKER_RUN_OPTS") \
  -h rabbitmq \
  -v rabbitmq-data:/var/lib/rabbitmq \
  -e RABBITMQ_DEFAULT_USER='admin' \
  -e RABBITMQ_DEFAULT_PASS='admin' \
  -p 5672:5672 \
  --name rabbitmq \
  docker.io/library/rabbitmq:3.8.2-alpine
```

```sh
echo -e '[INFO]\thttp://127.0.0.1:5672'
```

```sh
# Management
docker run -d \
  $(echo "$DOCKER_RUN_OPTS") \
  -h rabbitmq \
  -v rabbitmq-data:/var/lib/rabbitmq \
  -e RABBITMQ_DEFAULT_USER='admin' \
  -e RABBITMQ_DEFAULT_PASS='admin' \
  -p 5672:5672 \
  -p 15672:15672 \
  --name rabbitmq \
  docker.io/library/rabbitmq:3.8.2-management-alpine
```

> Wait! This process take a while.

```sh
echo -e '[INFO]\thttp://127.0.0.1:15672'
```

| Login | Password |
| --- | --- |
| guest | guest |
| admin | admin |

### Remove

```sh
docker rm -f rabbitmq
```
