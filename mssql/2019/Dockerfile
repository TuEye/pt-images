# -----------------------------------------------------
# MSSQL Image for Pterodactyl
# -----------------------------------------------------
FROM        --platform=$TARGETOS/$TARGETARCH mcr.microsoft.com/mssql/server:2019-latest

LABEL       author="TuEye" maintainer="tueye@tuworld.de"

ENV         DEBIAN_FRONTEND noninteractive

RUN         apt update -y \
						&& apt upgrade -y \
            && apt install -y netcat \
            && useradd -d /home/container -m container -s /bin/bash

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ../entrypoint.sh /entrypoint.sh
CMD ["/bin/bash", "/entrypoint.sh"]