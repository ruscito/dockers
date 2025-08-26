FROM node:20-bullseye

# ---- Config ----
ARG USERNAME=dev
ARG USER_UID=1001
ARG USER_GID=1001
ENV VIMRC_URL="https://gist.githubusercontent.com/ruscito/6ae612bb22680dfb51a128d6915f1471/raw/8f9d4bacab42ccda656bceae4dc4d91366fd4906/.vimrc"
# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    vim \
    sudo \
    openjdk-17-jdk \
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
    && mkdir -p "/home/${USERNAME}/.vim" \
    && mkdir -p "/home/${USERNAME}/.ssh" \
    && chmod 700 "/home/${USERNAME}/.ssh"

# ---- Setup SSH directory with proper permissions ----
RUN touch "/home/${USERNAME}/.ssh/known_hosts" \
    && chmod 644 "/home/${USERNAME}/.ssh/known_hosts" \
    && ssh-keyscan github.com >> "/home/${USERNAME}/.ssh/known_hosts" 2>/dev/null || true

# ---- Set workspace ----
WORKDIR /workspace

# ---- Default user ----
USER ${USERNAME}
CMD ["bash"]
