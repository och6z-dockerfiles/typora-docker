ARG IMAGE_VERSION

FROM debian:${IMAGE_VERSION}

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    gnupg \
    && wget -qO - https://typora.io/linux/public-key.asc | apt-key add - \
    && echo "deb https://typora.io/linux ./" > /etc/apt/sources.list.d/typora.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    typora \
    libglib2.0-0 \
    libx11-6 \
    libx11-xcb1 \
    libxcb-dri3-0 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxi6 \
    libxtst6 \
    libnss3 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    librust-gdk-pixbuf-sys-dev \
    libgtk-3-0 \
    libdrm2 \
    libgbm1 \
    libxss1 \
    libasound2 \
    libgl1 \
    && apt-get purge -y && apt-get autoremove -y && apt-get autoclean -y \
    && rm -rf /var/lib/apt/lists/*

ARG GID
ARG GID_NAME
ARG UID
ARG UID_NAME

RUN addgroup --gid ${GID} ${UID_NAME} \
    && adduser --uid ${UID} \
    --ingroup ${GID_NAME} \
    --home /home/${UID_NAME} \
    --shell /bin/bash \
    --disabled-password \
    --gecos "" ${UID_NAME}

ENV MESA_LOADER_DRIVER_OVERRIDE i965

USER ${UID_NAME}

WORKDIR /home/${UID_NAME}

ENTRYPOINT ["/bin/bash", "-c"]

CMD ["typora"]

#docker builder build --no-cache --build-arg IMAGE_VERSION=unstable-slim --build-arg GID=$(id -g) --build-arg GID_NAME=$(id -gn) --build-arg UID=$(id -u) --build-arg UID_NAME=$(id -un) --file Dockerfile.debian --tag image-name:version .
#xhost +local:docker
#docker container run --interactive --tty --privileged --volume /tmp/.X11-unix:/tmp/.X11-unix --env DISPLAY=unix$DISPLAY --name container-name image-name:version
