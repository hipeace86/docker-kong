# a minimal Nginx container including ContainerPilot and a simple virtulhost config
FROM nginx:latest

RUN apt-get update && apt-get install -y curl unzip

# we use consul-template to re-write our Nginx virtualhost config
RUN curl -Lo /tmp/consul_template_0.14.0_linux_amd64.zip https://releases.hashicorp.com/consul-template/0.14.0/consul-template_0.14.0_linux_amd64.zip && \
    unzip /tmp/consul_template_0.14.0_linux_amd64.zip && \
    mv consul-template /bin && \
    rm -rf /tmp/*

# get ContainerPilot release
ENV CONTAINERPILOT_VERSION 2.7.2
RUN export CP_SHA1=e886899467ced6d7c76027d58c7f7554c2fb2bcc \
    && curl -Lso /tmp/containerpilot.tar.gz \
         "https://github.com/joyent/containerpilot/releases/download/${CONTAINERPILOT_VERSION}/containerpilot-${CONTAINERPILOT_VERSION}.tar.gz" \
    && echo "${CP_SHA1}  /tmp/containerpilot.tar.gz" | sha1sum -c \
    && tar zxf /tmp/containerpilot.tar.gz -C /bin \
    && rm /tmp/containerpilot.tar.gz

# add ContainerPilot configuration and onChange handler
COPY containerpilot.json /etc/containerpilot/containerpilot.json
COPY reload-nginx.sh /bin/reload-nginx.sh

# add Nginx virtualhost configuration
COPY nginx.conf /etc/nginx/nginx.conf

# add Nginx virtualhost template that we'll overwrite
COPY nginx.conf.ctmpl /etc/containerpilot/nginx.conf.ctmpl

EXPOSE 8000 8443 8001 1337
