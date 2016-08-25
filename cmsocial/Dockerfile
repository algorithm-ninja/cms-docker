FROM ubuntu:14.04
MAINTAINER Algorithm Ninja <algorithm@ninja>

# Whether we can spare some space or not
ARG NO_PASCAL=0

# Which specialization we want to install
ARG CMSOCIAL_REPO=algorithm-ninja/cmsocial
ARG CMSOCIAL_BRANCH=master

# The path where the customization files are to be found
ARG CONFIG_PATH=config

# The path, relative to the context, of the folder where the Dockerfile (and other files) are located
ARG DOCKERFILE_FOLDER=.

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

# Prepare for CMS (maybe this can be done without git clone)
RUN git clone --recursive https://github.com/cms-dev/cms.git /cms && \
    cd /cms && \
    pip install -r requirements.txt && \
    ./prerequisites.py install --as-root --no-conf -y

# CMSocial dependencies: nginx
RUN apt-get -y install nginx

# CMSocial dependencies: nodejs + npm
RUN curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
RUN apt-get -y install nodejs

# Update make (this will go away after ubuntu 14.04)
RUN apt-get -y install wget && \
    wget http://mirrors.kernel.org/ubuntu/pool/main/m/make-dfsg/make_4.0-8.2_amd64.deb && \
    dpkg -i make_4.0-8.2_amd64.deb

# Clone CMSocial
RUN git clone --branch $CMSOCIAL_BRANCH --depth 1 https://github.com/$CMSOCIAL_REPO.git /cmsocial

# Override configuration
ADD $CONFIG_PATH /cmsocial/config

# Install CMS and CMSocial
RUN cd /cmsocial && \
    pip install -r requirements.txt && \
    python setup.py install && \
    make PROD=1

COPY $DOCKERFILE_FOLDER/supervisord.conf /etc/supervisor/conf.d/cmsocial.conf
COPY $DOCKERFILE_FOLDER/nginx.conf /etc/nginx/conf.d/cmsocial.conf
COPY $DOCKERFILE_FOLDER/blocked_ips /etc/nginx/blocked_ips

EXPOSE 8888 8889 8890
WORKDIR /cmsocial
CMD cgroups-mount
CMD supervisord
