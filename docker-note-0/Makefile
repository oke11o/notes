#!make

ifneq (,$(wildcard ./.env))
    include ./.env
    export $(shell sed 's/=.*//' ./.env)
endif


docker-up:
	docker-compose up -d

docker-down:
	docker-compose down

docker-build-es-rus-phonetic:
	docker build -f ./Dockerfile_es \
			--ssh default --pull \
			--progress=plain \
			-t elastic_rus_phonetic:7.3.0 .