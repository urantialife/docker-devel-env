ARG BASE_IMAGE
FROM $BASE_IMAGE

LABEL maintainer="Bernd Doser <bernd.doser@braintwister.eu>"

ARG VERSION

RUN apt-get update \
 && apt-get install -y \
    apt-transport-https \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

RUN add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

RUN apt-get update \
 && apt-get install -y \
    docker-ce=5:$VERSION.* \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

ADD add_user_to_docker_group.sh /entrypoint.d/
