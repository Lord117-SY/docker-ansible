FROM ubuntu:24.04

SHELL ["/bin/bash", "--login", "-c"]
RUN mkdir -p /etc/ansible
COPY files/ansible.cfg /etc/ansible/ansible.cfg
COPY files/entrypoint.sh /entrypoint.sh

#update container
ENV TZ="Europe/PARIS" \
    DEBIAN_FRONTEND="noninteractive"
RUN apt-get update -y
#Setup ssh server
RUN apt-get install openssh-server openssh-client sudo sshpass -y
RUN service ssh start
#create ansible user
RUN useradd -m ansible_user -s /bin/bash -U
COPY files/ansible_user /etc/sudoers.d/ansible_user
COPY files/bashrc /home/ansible_user/.bashrc
RUN chown ansible_user:ansible_user /home/ansible_user/.bashrc
USER ansible_user
# Install miniconda
ENV CONDA_DIR="/home/ansible_user/conda"
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /home/ansible_user/conda && \
    rm ~/miniconda.sh
ENV PATH=$CONDA_DIR/bin:$PATH
#create conda env
RUN conda create -y -n ansible python==2.7
RUN echo "conda activate ansible" >> /home/ansible_user/.bashrc
RUN conda activate ansible && \
    pip install ansible==2.9
RUN conda activate ansible && \
    pip install ansible-lint
ENTRYPOINT [ "/entrypoint.sh" ]
