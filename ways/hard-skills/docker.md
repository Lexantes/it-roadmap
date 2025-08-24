В общем и целом, первые две ссылки дают базу.
# Ссылки
1. [Romnero](https://www.youtube.com/playlist?list=PLqVeG_R3qMSwjnkMUns_Yc4zF_PtUZmB-)
2. [Девопсим потихоньку](https://www.youtube.com/@oomkilled) (Хороший канал). [Видео 1](https://www.youtube.com/watch?v=t4PEoHAvf1A) и [Видео 2](https://www.youtube.com/watch?v=sEbYhNuDoww)
3. https://labs.iximiuz.com/tutorials/pitfalls-of-from-scratch-images
4. https://docs.docker.com/reference/dockerfile/ и https://docs.docker.com/build/building/best-practices/
# Темы для изучения
1. Почитай про best practice при написание dockerfile и использование контейнеризации.
2. Взять какой-нибудь проект и попробовать написать для него dockerfile (собрать из исходников в докере).
# Задания
## Задание 1. Исправить dockerfile
Дан вот такой dockerfile. Исправь его. И опиши почему ты так сделал.
```
FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y wget git gcc make sudo vim nano curl && \
    wget https://golang.org/dl/go1.12.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go1.12.linux-amd64.tar.gz && \
    rm go1.12.linux-amd64.tar.gz
    
ENV PATH="/usr/local/go/bin:$PATH:/root/go/bin"

USER root

COPY . /app

WORKDIR /app/src

RUN go build -gcflags="all=-N -l" -o /app/bad-app

EXPOSE 1-65535

CMD ["/bin/bash"]
```
## Задание 2. Написание Dockerfile для разных приложений
Найти приложение на Python, Golang, Node.JS (на github их много), написать для них Dockerfile и собрать их docker образы. Запустить и проверить, что приложение работают корректно.