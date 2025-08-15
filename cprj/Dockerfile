# C + Vim development image with root + dev user, .vimrc from Gist (persistent)
FROM debian:bookworm-slim

# ---- Config ----
# Replace with your RAW Gist URL for .vimrc
ENV VIMRC_URL="https://gist.githubusercontent.com/ruscito/6ae612bb22680dfb51a128d6915f1471/raw/64652e9c101d20c5c9b3ef1ab2207072ad9613e5/.vimrc"
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000

# ---- Base packages ----
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    build-essential \
    gcc g++ \
    clang clang-format \
    gdb \
    make cmake \
    pkg-config \
    valgrind \
    git curl ca-certificates \
    vim \
    less \
    sudo \
    openssh-client \
    && rm -rf /var/lib/apt/lists/*

# ---- Create non-root dev user ----
RUN groupadd --gid ${USER_GID} ${USERNAME} \
  && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
  && chsh -s /bin/bash ${USERNAME} \
  && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# ---- Install .vimrc from Gist only if it does not exist ----
USER ${USERNAME}
RUN if [ ! -f "/home/${USERNAME}/.vimrc" ]; then \
      curl -fsSL "${VIMRC_URL}" -o "/home/${USERNAME}/.vimrc"; \
    fi \
    && mkdir -p "/home/${USERNAME}/.vim"

# ---- Default workspace ----
WORKDIR /workspace

# ---- Default shell ----
CMD ["bash"]

