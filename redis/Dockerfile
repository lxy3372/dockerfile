From ubuntu:latest

MAINTAINER  riky.liu<me@rikyliu.com>

ADD sources.list /etc/apt/
RUN sed -i 's/http:\/\/archive\.ubuntu\.com\/ubuntu\//http:\/\/mirrors\.163\.com\/ubuntu\//g' /etc/apt/sources.list 

RUN apt-get clean
#更新软件源
RUN apt-get update 
RUN apt-get install -y openssh-server #安装SSH
RUN apt-get install vim -y #安装vim
RUN apt-get install -y redis-server redis-tools
#RUN sed -i 's/\/var\/log\/nginx\/access\.log;/\/data\/logs\/access\.log;/g' /etc/nginx/nginx.conf
#RUN sed -i 's/\/var\/log\/nginx\/error\.log;/\/data\/logs\/error\.log;/g' /etc/nginx/nginx.conf
#修复最新ubuntu nginx bug
#RUN echo 'fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;' >>/etc/nginx/fastcgi_params
#RUN echo 'fastcgi_param PATH_INFO $fastcgi_script_name;' >>/etc/nginx/fastcgi_params

##要正常启动SSH服务，需要目录/var/run/sshd存在，手动创建它
RUN mkdir -p /var/run/sshd 
RUN mkdir -p /root/.ssh 
RUN sed -ri 's/session required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd
COPY authorized_keys /root/.ssh/authorized_keys

RUN mkdir -p /data/logs/redis
RUN mkdir -p /data/redis
#COPY api.conf /etc/nginx/conf.d/api.conf
COPY run.sh /run.sh
#copy主机的run.sh到容器里面来
RUN chmod 755 /run.sh 
#给容器的执行文件授权
EXPOSE 22
EXPOSE 6379
CMD ["/run.sh"]
