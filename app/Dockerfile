FROM ubuntu:20.04 AS base

ENV DEBIAN_FRONTEND=noninteractive


RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    gosu \
    lxterminal \
    net-tools \
    openbox \
    procps \
    python3 \
    python3-pip \
    supervisor \
    tigervnc-standalone-server \
    unzip \
    wget \
    file \
    xdg-utils && \
    rm -rf /var/lib/apt/lists && \
    mkdir -p /usr/share/desktop-directories

RUN mkdir -p /usr/share/novnc && \
    cd /usr/share/novnc && \
    git clone https://github.com/novnc/noVNC.git && \
    cd noVNC && \
    git checkout 7485e82b72d4d1356d95ecca2d109cbf49908b9d && \
    mv * /usr/share/novnc/

COPY config/index.html /usr/share/novnc

RUN cd /usr/share/novnc/utils && git clone https://github.com/novnc/websockify websockify

RUN mkdir -p /tmp/.X11-unix
RUN chmod 1777 /tmp/.X11-unix

ENV SC_USER_NAME=scu
RUN groupadd --gid 8004 ${SC_USER_NAME} && \
    useradd --home-dir /home/${SC_USER_NAME} --shell /bin/bash --uid 8004 --gid 8004 ${SC_USER_NAME} && \
    mkdir -p /home/${SC_USER_NAME}

ENV DY_SIDECAR_PATH_INPUTS=/home/${SC_USER_NAME}/work/inputs
ENV DY_SIDECAR_PATH_OUTPUTS=/home/${SC_USER_NAME}/work/outputs

EXPOSE 12000
EXPOSE 8080

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

FROM base AS production

# Download and install fsl, get rid of source
RUN wget --no-check-certificate https://fsl.fmrib.ox.ac.uk/fsldownloads/fslinstaller.py && \
    python3 fslinstaller.py -d /usr/local/fsl -q && \
    rm -rf /usr/local/fsl/src && \
    rm -r /usr/local/fsl/fslpython/pkgs/*tar.bz2

# Set up FSL paths
ENV FSLDIR=/usr/local/fsl
ENV PATH=$PATH:$FSLDIR/bin
ENV LD_LIBRARY_PATH=$FSLDIR/fslpython/envs/fslpython/lib/
# Other variables can be sourced from $FSLDIR/etc/fslconf/fsl.sh 
ENV FSLWISH=$FSLDIR/bin/fslwish 
ENV FSLOUTPUTTYPE=NIFTI_GZ
ENV FSLTCLSH=$FSLDIR/bin/fsltclsh
ENV USER=${SC_USER_NAME}

COPY config/menu.xml /etc/xdg/openbox/
COPY config/supervisord.conf /etc/

## workind directory and permissions
WORKDIR /home/scu/work
COPY docker/ /docker

CMD ["sh", "-c", "/docker/boot.sh"]
