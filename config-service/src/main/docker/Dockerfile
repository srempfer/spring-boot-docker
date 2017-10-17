################# First Stage #################

FROM alpine:3.6

ARG SPRING_BOOT_FAT_JAR

ADD $SPRING_BOOT_FAT_JAR /app.jar

RUN sh -c 'mkdir /app'
RUN unzip /app.jar -d /app

################# Second Stage #################

FROM frolvlad/alpine-oraclejdk8:slim

ENV JAVA_OPTS ""

RUN sh -c 'mkdir /app'

VOLUME /tmp
VOLUME /logs
VOLUME /config

ENTRYPOINT exec java -Djava.security.egd=file:/dev/./urandom -Dspring.config.location=file:/config/ $JAVA_OPTS -cp /app org.springframework.boot.loader.JarLauncher

COPY --from=0 /app/org /app/org
COPY --from=0 /app/BOOT-INF/lib /app/BOOT-INF/lib
COPY --from=0 /app/BOOT-INF/classes /app/BOOT-INF/classes
COPY --from=0 /app/META-INF /app/META-INF
