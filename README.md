<h1>TCH-lab14</h1>
# Wojciech Dziedzic


# zad
głównie używamy lab12.99+14 
oraz końcówka wykład 10 wraz z dodatkiem<br/>
i wykład 11<br/>

bazujemy na Zadaniu 1<br/>

PRZED<br/>
Założyć konto na aws, wykład 10<br/>
założyć usera do aws cli<br/>
zainstalować aws cli, oraz skonfigurować go<br/>
zainstalować aws eb cli (elastic beanstalk)<br/>

do instalaci aws oraz aws eb cli  śledzimy przykłądy w pdf oraz polecam linki do dokumentacji - są w pdf<br/>
 eb staramy sie zainstalować manualnie<br/>
do ew pomocy przy instalacji aws eb cli<br/>
https://www.youtube.com/watch?v=LcIpHF7JbZw <br/>
https://gist.github.com/navid-taheri/7d6879df3446b38b864fcb7ac1b04ab3 <br/>
potrzebny jest też do tego python oraz pip<br/>

zainstalować gh cli (github) (jak nie ma)<br/>

założyć repo na githubie <br/>

dodać do niego plik .github/workflows/main.yml<br/>

do tego repo należy dodać sekrety - token do dockerhuba oraz do aws<br/>
DOCKERHUB_USERNAME<br/>
DOCKERHUB_TOKEN<br/>

je bezpośrednio bota którego tworzymy jako nowege usera w koncie na aws<br/>
AWS_ACCESS_KEY_ID <br/>
AWS_SECRET_ACCESS_KEY<br/>

zalogować sie na dockerhub<br/>


najpierw tworzymy nowe repo na Dockerhubie np. voiteckheir/tch-lab14<br/>


w CMD<br/>
Gdy jesteśmy w katalogu z zadaniem<br/>

do uruchomienia maszyny EB<br/>

`eb init`<br/>

`eb create zadanie14-env`<br/>

`gh workflow list`<br/>

`gh workflow run <i>numer_workflowa</i>`<br/>

poda komende jak to podglądać, ale można tez an przeglądarce - github action
coś jak gh run list --workflow=main.yml


zostawiam rzeczy z poprzedniego zadania na zaś<br/>

# Część 1 (Obowiązkowa)
## 1.1
<b>Widok strony</b>
![image](https://github.com/VoiteckHeira/nginx-test-app/assets/91530837/e7aa46af-c865-4613-9683-e72f82de42d8)



## 1.2

``` Dockerfile
# syntax=docker/dockerfile:1.4

FROM  node:alpine3.16 as build-stage 

ARG VERSION

WORKDIR /app 

ENV REACT_APP_VERSION=${VERSION}

COPY package*.json /app/ 

RUN npm install axios && npm install && rm -rf $npm_config_cache && npm cache clean --force

COPY . . 

RUN npm run build


FROM nginx:1.23.4-alpine

ARG VERSION

LABEL org.opencontaine rs.image.version="$VERSION"
LABEL org.opencontainers.image.authors="Wojciech Dziedzic"

COPY nginx.conf /etc/nginx/conf.d/default.conf 

COPY --from=build-stage /app/build /usr/share/nginx/html 

EXPOSE 8080

HEALTHCHECK --interval=10s --timeout=1s \
    CMD curl -f http://localhost:8080/ || exit 1

CMD ["nginx", "-g", "daemon off;"]

```

## 1.3

```shell
docker build --build-arg VERSION=1.1 -t tchzad1:v1 .
```
![image](https://github.com/VoiteckHeira/nginx-test-app/assets/91530837/e191f375-1057-41c6-91e2-5bac96c1b38b)

```shell
docker run -d -p 8080:80 --name zad1cz1 tchzad1:v1
```
![image](https://github.com/VoiteckHeira/nginx-test-app/assets/91530837/a99a81bb-dc87-48fe-a8ce-cdad64dff36c)

```shell
docker history tchzad1:v1
```
![image](https://github.com/VoiteckHeira/nginx-test-app/assets/91530837/784808cd-6a4b-4be2-8bac-370269547833)

```shell
docker logs zad1cz1
```
![image](https://github.com/VoiteckHeira/nginx-test-app/assets/91530837/b8005570-61fb-438f-93a2-25fc2de3bef2)


<i>Ewentualne komendy do spokojnego zastopowania i usunięcia kontenera</i>
```shell
docker stop zad1cz1
```
```shell
docker rm zad1cz1
```
 # Część 2 (Dodatkowa)
