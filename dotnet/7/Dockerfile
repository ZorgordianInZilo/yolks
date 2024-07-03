FROM        --platform=$TARGETOS/$TARGETARCH mcr.microsoft.com/dotnet/sdk:7.0

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        ./../entrypoint.sh /entrypoint.sh

CMD         ["/entrypoint.sh"]
