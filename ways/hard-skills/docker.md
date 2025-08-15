В общем и целом, два этих видоса достаточно.
# Ссылки
1. [Romnero](https://www.youtube.com/playlist?list=PLqVeG_R3qMSwjnkMUns_Yc4zF_PtUZmB-)
2. [Девопсим потихоньку](https://www.youtube.com/watch?v=t4PEoHAvf1A)
# Темы для изучения
1. Почитай про best practice при написание dockerfile и использование 
2. Взять какой-нибудь проект и попробовать написать для него dockerfile (собрать из исходников в докере).
# Задание 1. Исправить dockerile
Дан вот такой dockerile. Исправь его. И опиши почему ты так сделал.
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