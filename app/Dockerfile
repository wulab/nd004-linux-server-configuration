FROM ubuntu

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
        && apt-get install -y --no-install-recommends \
                apache2 \
                ca-certificates \
                finger \
                git \
                gosu \
                libapache2-mod-wsgi \
                openssh-server \
                postgresql \
                sudo \
                ufw \
                vim

ENV S6_OVERLAY_VERSION 1.21.2.2

ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C /

RUN mkdir -p /var/run/sshd
RUN mkdir -p /var/run/postgresql/9.5-main.pg_stat_tmp \
        && chown -R postgres:postgres /var/run/postgresql \
        && chmod 2777 /var/run/postgresql
RUN gosu postgres /etc/init.d/postgresql start \
        && gosu postgres psql --command "CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';" \
        && gosu postgres createdb -O catalog catalog

RUN groupadd grader \
        && useradd --gid grader --shell /bin/bash --create-home grader

COPY root /
COPY grader.pub /home/grader/.ssh/authorized_keys

ENTRYPOINT [ "/init" ]
