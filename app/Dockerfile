FROM ubuntu

# ~~~~~~~~~~~~~~~~~~~~
# PACKAGE INSTALLATION
# ~~~~~~~~~~~~~~~~~~~~

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

# ~~~~~~~~~~~~~~~~~
# DATABASE CREATION
# ~~~~~~~~~~~~~~~~~

RUN mkdir -p /var/run/sshd \
        && mkdir -p /var/run/postgresql/9.5-main.pg_stat_tmp \
        && chown -R postgres:postgres /var/run/postgresql \
        && chmod 2777 /var/run/postgresql \
        && gosu postgres /etc/init.d/postgresql start \
        && gosu postgres psql --command \
                "CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';" \
        && gosu postgres createdb -O catalog catalog

# ~~~~~~~~~~~~~~~~~~~~~~~~
# APPLICATION INSTALLATION
# ~~~~~~~~~~~~~~~~~~~~~~~~

ENV FLASK_APP itemcatalog
ENV LANG C.UTF-8
RUN cd /usr/src \
        && git clone https://github.com/wulab/nd004-build-an-item-catalog.git \
        && cd /usr/src/nd004-build-an-item-catalog/app \
        && pip3 install --no-cache-dir -U pip setuptools \
        && pip3 install --no-cache-dir -e . \
        && flask initdb

# ~~~~~~~~~~~~
# USER ACCOUNT
# ~~~~~~~~~~~~

RUN groupadd grader \
        && useradd --gid grader --shell /bin/bash --create-home grader
COPY grader.pub /home/grader/.ssh/authorized_keys
RUN chown -R grader:grader /home/grader/.ssh \
        && chmod 700 /home/grader/.ssh \
        && chmod 600 /home/grader/.ssh/authorized_keys

# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# PROCESS SUPERVISION AND MANAGEMENT
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ENV S6_OVERLAY_VERSION 1.21.2.2
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C /
COPY root /

ENTRYPOINT [ "/init" ]
