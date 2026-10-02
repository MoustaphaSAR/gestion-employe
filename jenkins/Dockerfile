FROM jenkins/jenkins:lts

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends docker-cli ca-certificates curl \
    && mkdir -p /usr/local/lib/docker/cli-plugins \
    && curl -fsSL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 \
        -o /usr/local/lib/docker/cli-plugins/docker-compose \
    && chmod +x /usr/local/lib/docker/cli-plugins/docker-compose \
    && rm -rf /var/lib/apt/lists/*