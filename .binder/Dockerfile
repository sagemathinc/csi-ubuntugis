FROM ubuntu:18.04
LABEL maintainer="Harald Schilly <hsy@sagemath.com>"
ENV DEBIAN_FRONTEND=noninteractive
SHELL ["/bin/bash", "-c"]

ARG NB_USER
ARG NB_UID
ENV USER ${NB_USER}
ENV HOME /home/${NB_USER}

USER root

# NOTES
# installs from https://launchpad.net/~ubuntugis/+archive/ubuntu/ubuntugis-unstable

RUN apt-get update \
 && apt-get install -yq \
      curl \
      wget \
      bzip2 \
      less \
      vim \
      ca-certificates \
      locales \
      sudo \
      fonts-liberation \
      run-one \
      python3-dev \
      python3-pip \
      build-essential \
      python3-setuptools \
      python3-wheel \
      lsb-release \
      aptitude \
      software-properties-common \
      gpg \
  && add-apt-repository -y "ppa:ubuntugis/ubuntugis-unstable" \
  && gpg --keyserver keyserver.ubuntu.com --recv-keys 6B827C12C2D425E227EDCA75089EBE08314DF160 \
  && gpg -a --export 6B827C12C2D425E227EDCA75089EBE08314DF160 | apt-key add - \
  && gpg --keyserver keyserver.ubuntu.com --recv-keys 51F523511C7028C3 \
  && gpg -a --export 51F523511C7028C3 | apt-key add - \
  && add-apt-repository -y "deb https://qgis.org/ubuntu/ bionic main" \
  && apt-get update \
  && aptitude install -y grass qgis pycsw pygeoapi pyspatialite pystac \
     python-affine python-cartopy python-cligj python-django python-geopan python-mapnik python-pyproj python-shapel \
     pywps qgis r-cran-sp rasterio saga proj cython libgdal-grass python3-gdal \
  && apt-get clean && apt-get autoremove -y && rm -rf /var/lib/apt/lists/* \
  && pip3 install --no-cache \
      notebook \
      jupyterlab \
      numpy \
      scipy \
      pandas \
      seaborn \
      scikit-learn \
      sympy \
      statsmodels \
      matplotlib \
      yapf

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

# installing extras
COPY .binder/extra.sh /extra.sh
RUN bash /extra.sh && rm /extra.sh

RUN adduser --disabled-password \
    --gecos "User" \
    --uid ${NB_UID} \
    ${NB_USER}
WORKDIR ${HOME}
USER ${NB_USER}
