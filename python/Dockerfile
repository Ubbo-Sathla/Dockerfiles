ARG BASE_IMAGE

FROM ${BASE_IMAGE:-centos:7.9.2009} AS python_builder

ARG PYTHON_VERSION
ARG PROJECT_NAME

ENV LC_ALL en_US.utf8

ENV PYTHON_VERSION 3.6.15
ENV PROJECT_NAME kolla-rocky


ENV PYENV_ROOT /opt/pyenv
ENV PATH $PYENV_ROOT/bin:$PATH

#ENV PATH $PYENV_ROOT/shims:$PATH
#ENV PATH $PYENV_ROOT/plugins/pyenv-virtualenv/bin:$PATH
#ENV PATH $PYENV_ROOT/plugins/pyenv-virtualenv/shims:$PATH

ENV VIRTUAL_ENV $PYENV_ROOT/versions/$PYTHON_VERSION/envs/$PROJECT_NAME
ENV PATH $VIRTUAL_ENV/bin:$PATH
ENV PS1 "($PROJECT_NAME) ${PS1:-}"

# Install packages
RUN  yum  install -y make gcc patch git bzip2 fontconfig zlib-devel bzip2-devel openssl-devel ncurses-devel \
     sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel && yum clean all


RUN  \
    # Install pyenv
    git clone  --depth 1 https://github.com/pyenv/pyenv.git $PYENV_ROOT && \
    # Install pyenv-virtualenv
    git clone  --depth 1  https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv && \
    # Install python
    pyenv install $PYTHON_VERSION && \
    # install virtual python
    pyenv virtualenv $PYTHON_VERSION $PROJECT_NAME

FROM python_builder

RUN pip install --no-cache-dir  --upgrade pip
RUN pip install --no-cache-dir  -U 'ansible<2.9'  kolla-ansible==7.2.1 && \
    pip install --no-cache-dir python-glanceclient==2.13.0 python-keystoneclient==4.2.0 python-neutronclient==7.2.1 python-novaclient==17.2.1 python-openstackclient==3.16.3 python-cinderclient==4.0.2