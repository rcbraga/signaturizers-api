FROM --platform=linux/amd64 continuumio/miniconda3


#Add user
RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 vscode

# # Update args in docker-compose.yaml to set the UID/GID of the "vscode" user.
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then groupmod --gid $USER_GID vscode && usermod --uid $USER_UID --gid $USER_GID vscode; fi

RUN apt-get -qq update && apt-get install wget libxrender1 -y\
    && conda install -y python=3.7  \
    && conda update conda \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && conda install -c conda-forge rdkit tensorflow=1.14.0\
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

ENV PATH /opt/conda/bin:$PATH

RUN conda clean --all -y

# Create app directory
WORKDIR /workspace

# Run container as non-root
USER 1000:1000

