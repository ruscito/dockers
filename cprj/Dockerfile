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
    openssh-server \
    iputils-ping \
    net-tools \
    dnsutils \
    telnet \
    wget \
    gnupg2 \
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

# ---- Git global configuration setup script ----
RUN echo '#!/bin/bash' > "/home/${USERNAME}/setup-git.sh" \
    && echo 'echo "Setting up Git configuration..."' >> "/home/${USERNAME}/setup-git.sh" \
    && echo 'read -p "Enter your Git username: " git_username' >> "/home/${USERNAME}/setup-git.sh" \
    && echo 'read -p "Enter your Git email: " git_email' >> "/home/${USERNAME}/setup-git.sh" \
    && echo 'git config --global user.name "$git_username"' >> "/home/${USERNAME}/setup-git.sh" \
    && echo 'git config --global user.email "$git_email"' >> "/home/${USERNAME}/setup-git.sh" \
    && echo 'echo "Git configured successfully!"' >> "/home/${USERNAME}/setup-git.sh" \
    && chmod +x "/home/${USERNAME}/setup-git.sh"

# ---- SSH key generation script ----
RUN echo '#!/bin/bash' > "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo "Generating SSH key for GitHub..."' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'read -p "Enter your email for SSH key: " ssh_email' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'ssh-keygen -t ed25519 -C "$ssh_email" -f ~/.ssh/id_ed25519 -N ""' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo "SSH key generated! Public key:"' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'cat ~/.ssh/id_ed25519.pub' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo ""' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo "Copy the above public key and add it to your GitHub SSH keys:"' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo "https://github.com/settings/ssh/new"' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo ""' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && echo 'echo "Test your SSH connection with: ssh -T git@github.com"' >> "/home/${USERNAME}/generate-ssh-key.sh" \
    && chmod +x "/home/${USERNAME}/generate-ssh-key.sh"

# ---- Network test script ----
RUN echo '#!/bin/bash' > "/home/${USERNAME}/test-network.sh" \
    && echo 'echo "Testing network connectivity..."' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo "Gateway:"' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'ip route | grep default' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo ""' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo "Network interfaces:"' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'ip addr show' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo ""' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo "Testing ping to gateway:"' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'ping -c 3 $(ip route | grep default | awk "{print \$3}")' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo ""' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'echo "Testing external connectivity:"' >> "/home/${USERNAME}/test-network.sh" \
    && echo 'ping -c 3 8.8.8.8' >> "/home/${USERNAME}/test-network.sh" \
    && chmod +x "/home/${USERNAME}/test-network.sh"

# ---- Default workspace ----
WORKDIR /workspace

# ---- Default shell ----
CMD ["bash"]
