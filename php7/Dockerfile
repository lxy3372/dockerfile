From ubuntu:latest

MAINTAINER  riky.liu<me@rikyliu.com>

#ADD sources.list /etc/apt/
RUN sed -i 's/http:\/\/archive\.ubuntu\.com\/ubuntu\//http:\/\/mirrors\.163\.com\/ubuntu\//g' /etc/apt/sources.list 
RUN apt-get clean
RUN apt-get update
RUN apt-get install -y language-pack-en-base
RUN apt-get install -y software-properties-common
RUN LC_ALL=en_US.UTF-8  add-apt-repository -y ppa:ondrej/php
RUN apt-get update
RUN apt-get install -y openssh-server #安装SSH
RUN apt-get install vim -y #安装vim
#RUN apt-get install -y nginx
RUN apt-get install -y php7.2-fpm \
		php7.2-xml \
		php7.2-zip \
		php7.2-opcache \
		php7.2-soap \
		php7.2-mysql \
		php7.2-mbstring \
		php7.2-redis \
		php7.2-mongodb \
		php7.2-dev \
		php7.2-gd \
		php-pear \
		--fix-missing
RUN wget http://pear.php.net/go-pear.phar
RUN chmod 755 go-pear.phar
RUN pecl install swoole
RUN echo 'extension=swoole.so' >/etc/php/7.2/mods-available/swoole.ini
RUN ln -s /etc/php/7.2/mods-available/swoole.ini /etc/php/7.2/fpm/conf.d/20-swoole.ini
RUN ln -s /etc/php/7.2/mods-available/swoole.ini /etc/php/7.2/cli/conf.d/20-swoole.ini
#更换log目录
RUN sed -i 's/;access\.log = log\/\$pool\.access\.log/access\.log\ =\ \/data\/logs\/\$pool\.access\.log/g' /etc/php/7.2/fpm/pool.d/www.conf
RUN sed -i 's/;slowlog = log\/\$pool\.log\.slow/access\.log\ =\ \/data\/logs\/\$pool\.log\.slow/g' /etc/php/7.2/fpm/pool.d/www.conf
RUN sed -i 's/error_log = \/var\/log\/php7\.2-fpm\.log/error_log = \/data\/logs\/php7\.2-fpm\.log/g' /etc/php/7.2/fpm/php-fpm.conf

##要正常启动SSH服务，需要目录/var/run/sshd存在，手动创建它
RUN mkdir -p /var/run/sshd 
RUN mkdir -p /root/.ssh 
RUN sed -ri 's/session required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd
COPY authorized_keys /root/.ssh/authorized_keys 

RUN mkdir -p /data/logs
RUN mkdir -p /data/www
RUN echo "<h1>Welcome to nginx!</h1>" > /data/www/index.html
#RUN rm /etc/nginx/sites-enabled/default
#RUN rm /etc/nginx/sites-available/default
#COPY api.conf /etc/nginx/conf.d/api.conf
COPY run.sh /run.sh
#copy主机的run.sh到容器里面来
RUN chmod 755 /run.sh 
#给容器的执行文件授权
EXPOSE 22
CMD ["/run.sh"]
