# docker run -d -P --rm  -v ~/test:/opt --name t4 test4
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
	&& rm -rf /var/lib/apt/lists/*

ENV HOME /home/docker

RUN mkdir -p /var/run/sshd &&\
    mkdir -p ${HOME}/.ssh


RUN useradd --create-home --home-dir $HOME docker \
	&& chown -R docker:docker $HOME

# Set root password
RUN    echo 'root:0penbmc' >> /root/passwdfile

RUN    echo 'docker:docker' >> /root/passwdfile

# Apply root password
RUN    chpasswd -c SHA512 < /root/passwdfile && \
       rm /root/passwdfile

#RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/'  /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
#RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
#ENV NOTVISIBLE "in users profile"
#RUN echo "export VISIBLE=now" >> /etc/profile


#echo 'root:yourpasswd' | chpasswd //设置root密码
EXPOSE 22

USER docker

WORKDIR ${HOME}

COPY start.sh id_rsa.pub /

# add 172.17.0.2  63448863ac39 to host /etc/hosts
RUN cat /id_rsa.pub >> ${HOME}/.ssh/authorized_keys



#ENTRYPOINT service ssh restart && bash

ENTRYPOINT [ "/start.sh" ]