FROM nginx:stable
ENV DOCKER_CONTENT_TRUST = 1
USER root
COPY ./part3_mini_server.c /home/part3_mini_server.c
COPY ./nginx.conf /etc/nginx/nginx.conf
RUN apt-get update \
	&& apt-get install -y gcc \
	&& apt-get install -y spawn-fcgi \
	&& apt-get install -y libfcgi-dev \
	&& useradd tnj_container \
	&& chmod u-s /usr/sbin/unix_chkpwd \
	&& chmod u-s /usr/bin/chsh \
	&& chmod u-s /usr/bin/mount \
	&& chmod u-s /usr/bin/passwd \
	&& chmod u-s /usr/bin/su \
	&& chmod g-s /usr/sbin/unix_chkpwd \
	&& chmod u-s /usr/bin/chfn \
	&& chmod u-s /usr/bin/gpasswd \
	&& chmod g-s /usr/bin/expiry \
	&& chmod u-s /usr/bin/newgrp \
	&& chmod g-s /usr/bin/chage \
	&& chmod u-s /usr/bin/umount \
	&& chown tnj_container /home/part3_mini_server.c /etc/nginx/nginx.conf \
	#&& find / -perm /6000 -type f -exec ls -l {} \; \
	&& rm -rf /var/lib/apt/lists 
USER tnj_container
WORKDIR /home/
ENV DOCKER_CONTENT_TRUST=1
HEALTHCHECK --interval=5m --timeout=3s \
	CMD curl -f http://localhost/ || exit 1
ENTRYPOINT	gcc part3_mini_server.c -lfcgi -o p3m_server && \
		spawn-fcgi -p 8080 ./p3m_server && \
		nginx -g 'daemon off;'
