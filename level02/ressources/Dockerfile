FROM ubuntu:22.04

ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    openssh-client \
    sudo \
    ca-certificates \
    curl \
    vim \
    nano \
    git \
    iproute2 \
    iputils-ping \
    netcat-openbsd \
  && rm -rf /var/lib/apt/lists/*

RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m -s /bin/bash ${USERNAME} \
    && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-${USERNAME}-nopasswd \
    && chmod 0440 /etc/sudoers.d/90-${USERNAME}-nopasswd

USER ${USERNAME}
WORKDIR /workspace

# Prepare SSH directory (permissions will be enforced when mounting)
RUN mkdir -p /home/${USERNAME}/.ssh && chmod 700 /home/${USERNAME}/.ssh

CMD ["/bin/bash"]


