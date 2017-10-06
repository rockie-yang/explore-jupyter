FROM jupyter/jupyterhub

RUN apt-get update \
  && apt-get install -y git curl sed vim libxml2-dev libxslt-dev libjpeg-dev zlib1g-dev libpng12-dev

# RUN curl https://bootstrap.pypa.io/get-pip.py | python

RUN pip install sparkmagic

RUN jupyter nbextension enable --py --sys-prefix widgetsnbextension 

RUN jupyter-kernelspec install sparkmagic/kernels/sparkkernel
RUN jupyter-kernelspec install sparkmagic/kernels/pysparkkernel
RUN jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
RUN jupyter-kernelspec install sparkmagic/kernels/sparkrkernel
RUN jupyter serverextension enable --py sparkmagic 

