# ----------------------------------
# Test-Image for Screeps
# ----------------------------------

FROM        ghcr.io/parkervcp/yolks:ubuntu

LABEL       author="TuEye" maintainer="tueye@tuworld.de"

ARG         DEBIAN_FRONTEND=noninteractive

# Install Required Packages
RUN         apt update \
              && apt upgrade -y \
              && apt install -y gnupg2 software-properties-common

# Install MongoDB
RUN	        wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | apt-key add - \
              && echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/5.0 multiverse" > /etc/apt/sources.list.d/mongodb-org-5.0.list \
							&& apt update \
							&& apt install -y mongodb-org

# Install Redis
RUN					add-apt-repository ppa:chris-lea/redis-server \
						  && apt update \
							&& apt install -y redis-server

# Install NodeJS
RUN					curl -sL https://deb.nodesource.com/setup_8.x | -E bash - \
						  && apt update \
							&& apt install -y nodejs


USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
