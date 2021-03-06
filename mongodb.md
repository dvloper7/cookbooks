# MongoDB

## References

- [MongoDB ObjectId ↔ Timestamp Converter](https://steveridout.github.io/mongo-object-time/)

## Helm

### References

- [Exposing TCP and UDP services](https://github.com/kubernetes/ingress-nginx/blob/master/docs/user-guide/exposing-tcp-udp-services.md)

### Install

```sh
kubectl create namespace mongodb
```

```sh
helm install stable/mongodb \
  -n mongodb \
  --namespace mongodb \
  --set ingress.enabled=true \
  --set ingress.hosts={mongodb.example.com}
```

### Secrets

```sh
kubectl get secret mongodb \
  -o jsonpath='{.data.mongodb-root-password}' \
  -n mongodb | \
    base64 --decode; echo
```

### NGINX Ingress

```sh
helm upgrade nginx-ingress stable/nginx-ingress -f <(yq w <(helm get values nginx-ingress) tcp.27017 mongodb/mongodb:27017)
```

### Delete

```sh
helm delete mongodb --purge
kubectl delete namespace mongodb --grace-period=0 --force
```

<!-- ```sh
helm get values nginx-ingress > ./current-values.yaml
```

Adjust `^tcp: ` value:

```sh
vim ./current-values.yaml
```

```sh
helm upgrade nginx-ingress stable/nginx-ingress -f ./current-values.yaml
```

```sh
rm ./current-values.yaml
``` -->

## Docker

### Running

```sh
docker run -d \
  $(echo "$DOCKER_RUN_OPTS") \
  -h mongo \
  -v mongo-data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=root \
  -p 27017:27017 \
  --name mongo \
  docker.io/library/mongo:4.0
```

### Client

```sh
docker run -it --rm \
  docker.io/library/mongo:4.0 mongo -h
```

### Remove

```sh
docker rm -f mongo
docker volume rm mongo-data
```

## CLI

### Installation

#### Homebrew

```sh
brew install mongodb/brew/mongodb-community
```

### Service

#### Homebrew

```sh
brew services start mongodb-community
```

### Commands

```sh
mongo -h
```

### Examples

#### Database Authentication

```sh
mongo \
  --host [hostname] \
  --port 27017 \
  -u [username] \
  -p [password] \
  --authenticationDatabase [db-name]
```

### Evaluate

```sh
mongo --eval 'printjson(db.serverStatus())'
```

### Heredoc

```sh
mongo [db-name] <<-EOSQL
[commands]
EOSQL
```
