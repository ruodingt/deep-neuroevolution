FROM python:3.6-stretch

# Install dependencies: tmux, wget, and redis
RUN apt-get update && \
    apt-get install -y tmux wget cmake && \
    wget http://download.redis.io/redis-stable.tar.gz && \
    tar xvzf redis-stable.tar.gz && \
    cd redis-stable && \
    make && \
    make install

ADD .. /root/deep-neuroevolution/

# Install python dependencies directly
# virutalenv doesn't make sense in a docker container 
RUN cd ~/deep-neuroevolution/ && \
    pip install -r requirements.txt


############################## add ssh server ########################################


ARG user
ARG password

RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo "$user:$password" | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
#RUN echo "prohibit-password/PermitRootLogin yes" >> /etc/ssh/sshd_config
#RUN echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

RUN apt-get update && apt-get install -y tmux

COPY requirement.txt requirement.txt
RUN pip3 install -r requirement.txt

EXPOSE 22
EXPOSE 6006
EXPOSE 8888


CMD ["/usr/sbin/sshd", "-D"]
CMD bash -c "cd /root/deep-neuroevolution && ./scripts/local_run_redis.sh"

