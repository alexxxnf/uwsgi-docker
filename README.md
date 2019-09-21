# uWSGI docker image

Simple docker image containing bare **uWSGI** and **python3** or **python2** plugin for it.
Based on Alpine Linux.

Image exposes port 3031.

App should go to `/usr/src/app`.

uWSGI configuration should be stored in `/etc/uwsgi/uwsgi.ini`.

## Example

### Dockerfile

```
FROM alexxxnf/uwsgi-python3:latest

COPY ./src .

RUN apk add --virtual build-deps gcc python3-dev musl-dev postgresql-dev \
  && pip3 install --no-cache-dir -r /usr/src/app/requirements.txt \
  && apk del build-deps \
  && apk add --no-cache libpq
```

### docker-compose.yml

```
version: '3.1'

services:
  app:
    image: alexxxnf/app:0.0.1
    ports:
      - "3031:3031"
    volumes:
      - "./uwsgi.ini:/etc/uwsgi/uwsgi.ini"
```
