FROM ubuntu:14.04
MAINTAINER Algorithm Ninja <algorithm@ninja>

RUN apt-get update
RUN apt-get -y install g++
#RUN apt-get -y install fpc
RUN apt-get -y install postgresql-client
RUN apt-get -y install gettext
RUN apt-get -y install python2.7
RUN apt-get -y install iso-codes
RUN apt-get -y install shared-mime-info
RUN apt-get -y install stl-manual
RUN apt-get -y install cgroup-lite
RUN apt-get -y install supervisor
RUN apt-get -y install python-pip

# This apt-get is necessary until the following adopt python wheels:
#   * psycopg2
#   * pycups
#   * PyYAML
# (And until python wheels will support Linux binaries)
RUN apt-get -y install python-dev
RUN apt-get -y install libpq-dev
RUN apt-get -y install libcups2-dev
RUN apt-get -y install libyaml-dev

RUN git clone --recursive https://github.com/cms-dev/cms.git /cms

RUN cd /cms && \
    pip install -r requirements.txt && \
    ./prerequisites.py install --as-root -y && \
    python setup.py install

RUN git clone https://github.com/algorithm-ninja/cmsocial.git /cmsocial

RUN cd /cmsocial && \
    pip install -r requirements.txt && \
    python setup.py install && \
    make ONLINE=1

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY nginx.conf /etc/nginx/conf.d/cmsocial.conf

EXPOSE 8888 8889 8890
WORKDIR /cmsocial
CMD cgroups-mount
CMD supervisord
