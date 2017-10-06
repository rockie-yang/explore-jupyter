FROM debian:jessie

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing \
  && apt-get install -y git curl grep sed dpkg vim \
  	wget bzip2 ca-certificates \
  	libxml2-dev libxslt-dev libjpeg-dev zlib1g-dev libpng12-dev \
  	libglib2.0-0 libxext6 libsm6 libxrender1


RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda2-5.0.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

ENV PATH /opt/conda/bin:$PATH

ENTRYPOINT [ "/usr/bin/tini", "--" ]

RUN apt-get install -y build-essential 

RUN git clone https://github.com/minrk/jupyterhub-pydata-2016.git \
	&& cd jupyterhub-pydata-2016 \
	&& conda env create -f environment.yml 

RUN ls -al /bin/sh \
	&& rm /bin/sh \
	&& ln -sf /bin/bash /bin/sh

RUN python --version \
	&& source activate jupyterhub-tutorial \
	&& python --version \
	&& conda install -y -c conda-forge jupyterhub 

RUN echo 'deb http://deb.debian.org/debian jessie-backports main' > /etc/apt/sources.list.d/backports.list

RUN apt-get update && apt-get install -y docker.io 

RUN docker --version

RUN source activate jupyterhub-tutorial \
	&& pip install dockerspawner 

# without the password change step, the user is refused to login in jupyterhub 
RUN useradd -ms /bin/bash -p user \
 	&& useradd -ms /bin/bash -p rockie \
 	&& useradd -ms /bin/bash -p test \
 	&& echo "test" | passwd --stdin test \
	&& echo "user" | passwd --stdin user \
	&& echo "rockie" | passwd --stdin rockie 

CMD [ "/bin/bash" ]
