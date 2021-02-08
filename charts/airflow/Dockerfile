FROM apache/airflow:1.10.14
USER root

RUN echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | debconf-set-selections

RUN set -ex \
    && buildDeps=' \
        freetds-dev \
        libkrb5-dev \
        libsasl2-dev \
        libssl-dev \
        libffi-dev \
        libpq-dev \
    ' \
    && apt-get update -yqq \
    && apt-get upgrade -yqq \
    && apt-get install -yqq --no-install-recommends \
        $buildDeps \
        git \
        freetds-bin \
        build-essential \
        default-libmysqlclient-dev \
        apt-utils \
        curl \
        rsync \
        netcat \
        locales \
        vim \
        nano \
        ldap-utils \
        unixodbc-dev \
        gnupg \
    && sed -i 's/^# en_US.UTF-8 UTF-8$/en_US.UTF-8 UTF-8/g' /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8 \
#    && useradd -ms /bin/bash -d ${AIRFLOW_USER_HOME} airflow \
    && pip install -U pip setuptools wheel \
    && pip install pytz \
    && pip install pyOpenSSL \
    && pip install ndg-httpsclient \
    && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update -yqq \
    && apt-get install msodbcsql17 -y \
    && apt-get install -y cifs-utils \
        && sed -i 's/^# en_US.UTF-8 UTF-8$/en_US.UTF-8 UTF-8/g' /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8 \
    && pip install apache-airflow-backport-providers-microsoft-azure==2020.11.23rc1 \
    && pip install -U pip setuptools wheel pyodbc \
    && pip install pyasn1 \
    && pip install SQLAlchemy==1.3.15

#USER airflow
#RUN pip install apache-airflow-backport-providers-microsoft-azure==2020.11.23rc1 \
#  && pip install -U pip setuptools wheel pyodbc \
#  && pip install SQLAlchemy==1.3.15 \
#  && pip install flask_bcrypt \
#  && pip install pyasn1