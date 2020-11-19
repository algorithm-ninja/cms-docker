FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update

# Dependencies
RUN apt-get install -y build-essential openjdk-8-jdk-headless fp-compiler \
    postgresql postgresql-client python3.6 cppreference-doc-en-html \
    cgroup-lite libcap-dev zip haskell-platform rustc mono-mcs
# Python Dependencies
RUN apt-get install -y python3-setuptools python3-tornado python3-psycopg2 \
     python3-sqlalchemy python3-psutil python3-netifaces python3-crypto \
     python3-six python3-bs4 python3-coverage python3-mock python3-requests \
     python3-werkzeug python3-gevent python3-bcrypt python3-chardet patool \
     python3-babel python3-xdg python3-future python3-jinja2

# This apt-get is necessary until the following adopt python wheels:
#   * psycopg2
#   * pycups
#   * PyYAML
# (And until python wheels will support Linux binaries)
#RUN apt-get -y install python-dev libpq-dev libcups2-dev libyaml-dev libffi-dev

# Tools
RUN apt-get -y install git

# Prepare for CMS
RUN git clone --recursive https://github.com/cms-dev/cms.git /cms && \
    cd /cms && \
    ./prerequisites.py --as-root --no-conf -y install && \
    python3 setup.py install

# Additional dependencies (to run multiple CWS instances)
RUN apt-get -y install nginx

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY nginx.conf /etc/nginx/conf.d/cms.conf
COPY blocked_ips /etc/nginx/blocked_ips

EXPOSE 8888 8889 8890
WORKDIR /cms
CMD cgroups-mount
CMD supervisord
