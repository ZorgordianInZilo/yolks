FROM        --platform=$TARGETOS/$TARGETARCH ghcr.io/parkervcp/yolks:ubuntu

LABEL       author="Torsten Widmann" maintainer="info@goover.de"

ENV         DEBIAN_FRONTEND noninteractive

RUN         apt update -y \
            && apt install -y apt-transport-https

RUN         apt upgrade -y \
            && apt install -y wget curl iproute2 libgdiplus tini libfontconfig1 libsm6 libice6

RUN         wget https://dot.net/v1/dotnet-install.sh \
            && D_V="$(curl -sSL https://dotnet.microsoft.com/en-us/download/dotnet/7.0 | grep -i  '<h3 id="sdk-7.*">SDK 7.*.*</h3>'  | head -1 | awk -F\" '{print $3}' | awk '{print $2;}' | sed 's/<\/h3>//g')" \ 
            && chmod +x dotnet-install.sh \
			&& ./dotnet-install.sh -i /usr/share -v $D_V \
            && ln -s /usr/share/dotnet /usr/bin/dotnet

#RUN         apt install -y ca-certificates libc6 libgcc-s1 libgssapi-krb5-2 libicu70 liblttng-ust1 libssl3 libstdc++6 libunwind8 zlib1g

# Required to make SkiaSharp 2.80.1 work. If use 2.80.3 SkiaSharp.NativeAssets.Linux.NoDependencies it is not required.
#RUN 		apt install --no-cache fontconfig

#RUN 		apt install --no-cache icu-libs krb5-libs libgcc libintl libssl1.1 libstdc++ zlib

# Add the missed ld-linux-x86-64.so.2 dependency.
RUN 		apt install --no-cache gcompat


USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
