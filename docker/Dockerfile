FROM ubuntu:18.04

MAINTAINER Kamil Kwiek <kamil.kwiek@continuum.io>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN echo 'started installing anaconda'

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget https://repo.continuum.io/archive/Anaconda2-5.1.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN echo 'finished installing anaconda'

RUN echo 'started second installation'
RUN apt-get install -y --fix-missing curl grep sed dpkg libav-tools && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

RUN echo 'finished second installation'

ENV PATH /opt/conda/bin:$PATH

#RUN conda create -n tensorflow python=2.7

#CMD ["source", "activate tensorflow"]

#RUN conda install -c conda-forge tensorflow

#RUN echo 'install KERAS'

#RUN conda install -c conda-forge keras

#RUN echo 'finish KERAS'

RUN pip install librosa np_utils pytube

RUN pip install tensorflow

RUN pip install keras

ENTRYPOINT [ "/usr/bin/tini", "--" ]
CMD [ "/bin/bash" ]
