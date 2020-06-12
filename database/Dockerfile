FROM postgres:latest

ADD . /chatapplication

WORKDIR /chatapplication

COPY createdb.sql /docker-entrypoint-initdb.d

