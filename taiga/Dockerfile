# Taiga in a single box

FROM debian:jessie

MAINTAINER geksiong@gmail.com

# Install dependencies
RUN \
  apt-get update && apt-get install -y \
    python3 python3-dev python3-setuptools python3-pip \
    git-core locales \
    postgresql-9.4 postgresql-server-dev-9.4 \
    libxml2-dev libxslt-dev \
    nginx \
  && easy_install3 requests \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# taiga-back
RUN \
  mkdir /taiga \
  && cd /taiga \
  && git clone https://github.com/taigaio/taiga-back.git \
  && cd taiga-back \
  && git checkout stable \
  && pip3 install -r requirements.txt \
  && pip3 install kombu \
  && pip3 install billiard 

# taiga-front
RUN \
  cd /taiga \
  && git clone https://github.com/taigaio/taiga-front-dist.git \
  && cd taiga-front-dist \
  && git checkout stable \
  && rm /etc/nginx/sites-enabled/default \
  && mkdir /taiga/logs

# config files
COPY nginx/sites-enabled/taiga /etc/nginx/sites-enabled/taiga
COPY taiga-back/settings/local.py /taiga/taiga-back/settings/local.py
COPY taiga-front-dist/dist/js/conf.json /taiga/taiga-front-dist/dist/js/conf.json

# initial bootstrap
RUN \
  service postgresql start \
  && su - postgres -c "createuser root" \
  && su - postgres -c "createdb taiga -O root" \
  && cd /taiga/taiga-back \
  && python3 manage.py migrate --noinput \
  && python3 manage.py loaddata initial_user \
  && python3 manage.py loaddata initial_project_templates \
  && python3 manage.py loaddata initial_role \
  && python3 manage.py collectstatic --noinput

EXPOSE 80 443

VOLUME ["/var/lib/postgresql/9.4/main"]

CMD service postgresql start && service nginx start && cd /taiga/taiga-back && python3 manage.py runserver

