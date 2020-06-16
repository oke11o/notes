# Собрать свой docker image

Пытаюсь сделать 

```shell script
docker-compose up -d
```
 
Получаю прикольные ошибки

```shell script
Creating network "devenv_default" with the default driver
Pulling es01 (elastic_rus_phonetic:7.3.0)...
ERROR: The image for the service you're trying to recreate has been removed. If you continue, volume data could be lost. Consider backing up your data before continuing.

Continue with the new image? [yN]y
Pulling es01 (elastic_rus_phonetic:7.3.0)...
ERROR: pull access denied for elastic_rus_phonetic, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
make: *** [docker-infra] Error 1
```


Типа найти не может контейнер. 
Конечно не может. Это же самодельный контейнер.
А как тогда собраться?

И тут смотрю. Есть классный файл Dockerfile_es

```dockerfile
FROM docker.elastic.co/elasticsearch/elasticsearch:7.3.0

RUN bin/elasticsearch-plugin install https://github.com/papahigh/elasticsearch-russian-phonetics/raw/7.3.0/esplugin/plugin-distributions/analysis-russian-phonetic-7.3.0.zip
```

ААААА. Так мы тут собираем образ для себя.

А нука посмотрим. В Makefile есть классная команда.

```shell script
docker build -f build/Dockerfile_es \
    --ssh default --pull \
    --progress=plain \
    -t elastic_rus_phonetic:7.3.0 .
```

И вот у нас есть нужный образ. Теперь можно спокойно делать `docker-compose up -d`

## Коротко и в Makefile

```shell script
# пытаемся поднять и не удается
make docker-up
# билдим образ
make docker-build-es-rus-phonetic
# успешно поднимаем
make docker-up
# убиваем контейнеры
make docker-down
```


