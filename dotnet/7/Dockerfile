FROM        --platform=$TARGETOS/$TARGETARCH ghcr.io/parkervcp/yolks:debian

LABEL       author="Torsten Widmann" maintainer="info@goover.de"

ENV         DEBIAN_FRONTEND noninteractive

# Required to make SkiaSharp 2.80.1 work. If use 2.80.3 SkiaSharp.NativeAssets.Linux.NoDependencies it is not required.
RUN 		apk add --no-cache fontconfig

RUN 		apk add --no-cache icu-libs krb5-libs libgcc libintl libssl1.1 libstdc++ zlib
# For debug purposes.
RUN 		apk add --no-cache bash
# Add the missed ld-linux-x86-64.so.2 dependency.
RUN 		apk add --no-cache gcompat

RUN         apt update -y \
            && apt upgrade -y \
            && apt install -y apt-transport-https wget curl iproute2 libgdiplus tini libfontconfig1\
            && wget https://dot.net/v1/dotnet-install.sh \
            && D_V="$(curl -sSL https://dotnet.microsoft.com/en-us/download/dotnet/7.0 | grep -i  '<h3 id="sdk-7.*">SDK 7.*.*</h3>'  | head -1 | awk -F\" '{print $3}' | awk '{print $2;}' | sed 's/<\/h3>//g')" \ 
            && chmod +x dotnet-install.sh \
			&& ./dotnet-install.sh -i /usr/share -v $D_V \
            && ln -s /usr/share/dotnet /usr/bin/dotnet

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
