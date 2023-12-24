FROM mcr.microsoft.com/dotnet/aspnet:7.0-jammy

SHELL ["/bin/bash", "-c"]

EXPOSE 5000

ENV ASPNETCORE_ENVIRONMENT="Production"
ENV ASPNETCORE_URLS="http://*:5000"

COPY ./DockerMain.sh /src/DockerMain.sh
COPY . /app

WORKDIR /app

RUN \
  apt-get -y update && \
  apt-get -y install curl && \
  mkdir -p /remotely-data && \
  sed -i 's/DataSource=Remotely.db/DataSource=\/remotely-data\/Remotely.db/' /app/appsettings.json

VOLUME "/remotely-data"

RUN chmod +x "/src/DockerMain.sh"

ENTRYPOINT ["/src/DockerMain.sh"]

HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost:5000/api/healthcheck || exit 1
