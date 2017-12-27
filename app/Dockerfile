FROM ubuntu

RUN apt-get update \
        && apt-get install -y --no-install-recommends \
                apache2 \
                ca-certificates \
                finger \
                git \
                gosu \
                libapache2-mod-wsgi-py3 \
                openssh-server \
                postgresql \
                python3-pip \
                sudo \
                ufw \
                vim

RUN mkdir -p /var/run/sshd
RUN mkdir -p /var/run/postgresql/9.5-main.pg_stat_tmp \
        && chown -R postgres:postgres /var/run/postgresql \
        && chmod 2777 /var/run/postgresql
RUN gosu postgres /etc/init.d/postgresql start \
        && gosu postgres psql --command "CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';" \
        && gosu postgres createdb -O catalog catalog

ENV LANG C.UTF-8
ENV FLASK_APP itemcatalog
WORKDIR /usr/src
RUN git clone https://github.com/wulab/nd004-build-an-item-catalog.git
WORKDIR /usr/src/nd004-build-an-item-catalog/app
RUN pip3 install --no-cache-dir -U pip setuptools \
        && pip3 install --no-cache-dir -e . \
        && flask initdb

RUN groupadd grader \
        && useradd --gid grader --shell /bin/bash --create-home grader
COPY grader.pub /home/grader/.ssh/authorized_keys
RUN chown -R grader:grader /home/grader/.ssh \
        && chmod 700 /home/grader/.ssh \
        && chmod 600 /home/grader/.ssh/authorized_keys

ENV S6_OVERLAY_VERSION 1.21.2.2
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C /

COPY root /

ENTRYPOINT [ "/init" ]
