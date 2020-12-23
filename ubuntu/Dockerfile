# docker run -d -P --rm  -v ~/test:/opt --name t4 test4
# sudo docker run -d -P --rm  -v /mnt/disk3:/opt --name u6 ubuntu1:v6
# 
FROM ubuntu:18.04

# environment settings
ENV DEBIAN_FRONTEND="noninteractive"

# install deps
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list


RUN  apt-get clean &&\
    apt-get update && apt-get install -y \
	ca-certificates \
	openssh-server \
    bash \
	build-essential \
	chrpath \
	curl \
	diffstat \
	gawk \
	git \
	libpixman-1-0 \
	libsdl1.2-dev \
	texinfo \
    python2.7\
	cpio\
	locales \
	tree \
	samba \
	samba-common\
	coreutils\
	gnome-shell\
	&& locale-gen "en_US.UTF-8"

RUN apt-get install -y sudo \
	&& echo "docker   ALL=(ALL)       ALL" >> /etc/sudoers

RUN apt-get install -y gnome-terminal

ENV HOME=/home/docker \
	LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
	OPENBMC="VERSION 2.8.0"\
	TEMPLATECONF=meta-ibm/meta-palmetto/conf

RUN mkdir -p /var/run/sshd \
    && mkdir -p ${HOME}/.ssh \
	&& mkdir -p /etc/samba
  

# Apply root password

RUN useradd --create-home --shell /bin/bash  docker \
	&& chown -R docker:docker ${HOME} \
    && echo 'root:0penBmc' >> /root/passwdfile \
    && echo 'docker:docker' >> /root/passwdfile \
	&& chpasswd -c SHA512 < /root/passwdfile && \
       rm /root/passwdfile

#RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/'  /etc/ssh/sshd_config

# write to .ssh/config
# Host huigher
# HostName 121.40.xxx.xxx
# Port 22
# User root
# IdentityFile /YourPath/YourPrivateKey


#echo 'root:yourpasswd' | chpasswd //设置root密码
EXPOSE 22

#USER docker

WORKDIR ${HOME}

COPY start.sh id_rsa.pub / 
COPY  smb.conf /etc/samba/

# add 172.17.0.2  63448863ac39 to host /etc/hosts
RUN ssh-keygen -t rsa -b 2048 -f /etc/ssh/ssh_root_rsa_key \
	&& cat /id_rsa.pub >> ${HOME}/.ssh/authorized_keys \
    && chmod -R 700 ${HOME}/.ssh \
    && chmod 600 ${HOME}/.ssh/authorized_keys




#ENTRYPOINT service ssh restart && bash
CMD [ "service smbd restart" ]
ENTRYPOINT [ "/start.sh" ]
