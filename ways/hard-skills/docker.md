В общем и целом, первые две ссылки дают базу.
# Ссылки
1. [Romnero](https://www.youtube.com/playlist?list=PLqVeG_R3qMSwjnkMUns_Yc4zF_PtUZmB-)
2. [Девопсим потихоньку](https://www.youtube.com/@oomkilled) (Хороший канал). [Видео 1](https://www.youtube.com/watch?v=t4PEoHAvf1A) и [Видео 2](https://www.youtube.com/watch?v=sEbYhNuDoww)
3. https://labs.iximiuz.com/tutorials/pitfalls-of-from-scratch-images
4. https://docs.docker.com/reference/dockerfile/ и https://docs.docker.com/build/building/best-practices/
5. [Как ускорить сборку Docker-образов в GitLab: стратегии кэширования с Docker Buildx](https://habr.com/ru/companies/bimeister/articles/854064/)
6. [Лонгрид](https://habr.com/ru/articles/935178/)
7. [Курс со степика](https://stepik.org/course/123300)
# Темы для изучения
1. Почитай про best practice при написание dockerfile и использование контейнеризации.
2. Взять какой-нибудь проект и попробовать написать для него dockerfile (собрать из исходников в докере).
# Best practice
1. https://docs.docker.com/build/building/best-practices/
2. Не используй latest tag. Фиксировать версию - хорошо.
3. Используй multi-stage и называй stage с помощью AS
4. В финальном stage старайся использовать distroless или scratch образы. Т.к. зачем тебе то, что не используется в образе? Можно ещё закидывать туда, что тебе надо, формирую отдельный stage с rootfs (но может не работать в некоторых сценариях из-за конфликтов). Что-то типо такого:
```Dockerfile
...
#---------------------------------------------------------------
FROM scratch AS rootfs
WORKDIR /rootfs
COPY --from=build /opt/openssl/bin/ bin/
COPY --from=build /opt/openssl/lib64/ lib/
COPY --from=build \
     /lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 \
     lib64/
COPY --from=build \
     /lib/x86_64-linux-gnu/libc.so.6 \
     lib/
COPY --from=build \
    /opt/openssl/include/ usr/include/
#---------------------------------------------------------------
FROM scratch AS final
LABEL org.opencontainers.image.base.name="scratch"
COPY --from=rootfs /rootfs /
```
5. Используй лейблы (помогают при дебаге, особенно коммит). Можно формат такой [ссылка](https://specs.opencontainers.org/image-spec/annotations/?v=v1.0.1). Думаю, можно начать с:
```
org.opencontainers.image.authors
org.opencontainers.image.created
org.opencontainers.image.version
org.opencontainers.image.commit
```
# Задания
## Задание 1. Исправить dockerfile
Дан вот такой dockerfile. Исправь его. И опиши почему ты так сделал.
```Dockerfile
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