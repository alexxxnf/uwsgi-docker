FROM alpine:3.7

EXPOSE 3031

WORKDIR /usr/src/app

RUN apk add --no-cache \
        uwsgi-python \
        python2

COPY ./uwsgi.ini /etc/uwsgi/uwsgi.ini

CMD ["uwsgi", "--ini", "/etc/uwsgi/uwsgi.ini"]
