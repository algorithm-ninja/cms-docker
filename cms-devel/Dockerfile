FROM ubuntu:14.04
MAINTAINER Algorithm Ninja <algorithm@ninja>

ARG NO_PASCAL=0

RUN apt-get update

# Dependencies
RUN apt-get -y install postgresql-client gettext python2.7 iso-codes shared-mime-info stl-manual cgroup-lite supervisor

# Compilers (for supported languages)
RUN apt-get -y install g++
RUN if test "$NO_PASCAL" = '0'; then apt-get -y install fpc; fi

# This apt-get is necessary until the following adopt python wheels:
#   * psycopg2
#   * pycups
#   * PyYAML
# (And until python wheels will support Linux binaries)
RUN apt-get -y install python-dev libpq-dev libcups2-dev libyaml-dev libffi-dev

# Tools
RUN apt-get -y install git python-pip

# Prepare for CMS
RUN cd /cms && \
    pip install -r requirements.txt && \
    ./prerequisites.py install --as-root --no-conf -y && \
    python setup.py install

# Additional dependencies (to run multiple CWS instances)
RUN apt-get -y install nginx

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY nginx.conf /etc/nginx/conf.d/cms.conf
COPY blocked_ips /etc/nginx/blocked_ips

EXPOSE 8888 8889 8890
WORKDIR /cms
CMD cgroups-mount
CMD supervisord
