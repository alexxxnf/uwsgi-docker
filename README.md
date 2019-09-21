# uWSGI docker image

Simple docker image containing bare **uWSGI** and **python3** or **python2** plugin for it.
Based on Alpine Linux.

Image exposes port 3031 and uses the uwsgi protocol by default.

App should go to `/usr/src/app`.

uWSGI configuration should be stored in `/etc/uwsgi/uwsgi.ini`.

## Example

### Simple Dockerfile

```
FROM alexxxnf/uwsgi-python3:latest

COPY ./src .

RUN apk add --virtual build-deps gcc python3-dev musl-dev postgresql-dev \
  && pip3 install --no-cache-dir -r /usr/src/app/requirements.txt \
  && apk del build-deps \
  && apk add --no-cache libpq
```

### Multistage Dockerfile

```
FROM alexxxnf/uwsgi-python2:latest as builder

WORKDIR /requirements

COPY requirements.conf /requirements.txt

RUN apk add gcc python2-dev py-pip musl-dev postgresql-dev \
  && pip2 install --install-option="--prefix=/requirements" --no-cache-dir -r /requirements.txt

FROM alexxxnf/uwsgi-python2:latest

RUN apk add --no-cache libpq

COPY --from=builder /requirements /usr
COPY ./src .
```

### docker-compose.yml

```
version: '3.1'

services:
  app:
    image: alexxxnf/app:0.0.1
    ports:
      - "3031:3031"
```
