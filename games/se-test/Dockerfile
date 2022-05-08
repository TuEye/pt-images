# ----------------------------------
# Wine 6 image for SE
# ----------------------------------
FROM    ghcr.io/parkervcp/yolks:debian

LABEL   author="Michael Parker" maintainer="parker@pterodactyl.io"

## install required packages
RUN     dpkg --add-architecture i386 \
        && apt update -y \
        && apt install -y --no-install-recommends gnupg2 software-properties-common libntlm0 winbind xvfb xauth python3 libncurses5:i386 libncurses6:i386 expect lsb-release

# Install winehq-stable and with recommends
ARG     WineVersion="6.0.4~bullseye-1"
RUN     wget -nc https://dl.winehq.org/wine-builds/winehq.key \
        && apt-key add winehq.key \
        && echo "deb https://dl.winehq.org/wine-builds/debian/ bullseye main" >> /etc/apt/sources.list \
        && wget -O- -q download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_11/Release.key | apt-key add - \
        && echo "deb http://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_11 ./" | tee /etc/apt/sources.list.d/wine-obs.list \
        && apt update \
        && apt install -y --install-recommends cabextract xvfb winehq-stable=$WineVersion wine-stable=$WineVersion wine-stable-amd64=$WineVersion wine-stable-i386=$WineVersion

# Set up Winetricks
RUN	    wget -q -O /usr/sbin/winetricks https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks \
        && chmod +x /usr/sbin/winetricks

ENV     HOME=/home/container
ENV     WINEPREFIX=/home/container/.wine
ENV     WINEDLLOVERRIDES="mscoree=d;mshtml=d"
ENV     DISPLAY=:0.0
ENV     DISPLAY_WIDTH=1024
ENV     DISPLAY_HEIGHT=768
ENV     DISPLAY_DEPTH=16
ENV     AUTO_UPDATE=1
ENV     XVFB=1

# Install needed Winetricks
ENV			WINEARCH=win64
#ENV			WINEDEBUG=-all

RUN			su container -c 'Xvfb :0 -screen 0 1024x768x16' & \
				su container -c 'wineboot --init /nogui' \
				&& su container -c '/usr/sbin/winetricks corefonts' \
				&& su container -c '/usr/sbin/winetricks -q vcrun2017' \
				&& su container -c '/usr/sbin/winetricks -q --force dotnet48' \
				&& su container -c '/usr/sbin/winetricks sound=disabled' \
				&& su container -c '/usr/sbin/winetricks -q vcrun2013' \
				&& su container -c 'rm -rf ~/.cache'

USER    container
WORKDIR	/home/container

COPY    ./entrypoint.sh /entrypoint.sh
CMD	    ["/bin/bash", "/entrypoint.sh"]
