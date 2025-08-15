# Base image: Python 3.11 slim
FROM python:3.11-slim

# ---- Config ----
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000
ENV VIMRC_URL="https://gist.githubusercontent.com/ruscito/6ae612bb22680dfb51a128d6915f1471/raw/64652e9c101d20c5c9b3ef1ab2207072ad9613e5/.vimrc"

# ---- Install system packages ----
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git curl ca-certificates sudo vim less \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# ---- Create non-root dev user ----
RUN groupadd --gid ${USER_GID} ${USERNAME} \
  && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
  && chsh -s /bin/bash ${USERNAME} \
  && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# ---- Python packages ----
RUN python -m pip install --upgrade pip setuptools wheel \
    && pip install --no-cache-dir \
       numpy pandas matplotlib scikit-learn \
       jupyterlab notebook tensorflow keras seaborn plotly

# ---- Install .vimrc from Gist for both root and dev ----
RUN curl -fsSL "${VIMRC_URL}" -o "/root/.vimrc" \
  && mkdir -p "/root/.vim" \
  && cp /root/.vimrc "/home/${USERNAME}/.vimrc" \
  && mkdir -p "/home/${USERNAME}/.vim" \
  && chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}/.vim*

# ---- Set workspace ----
WORKDIR /workspace

# ---- Default user ----
USER ${USERNAME}

# ---- Default command ----
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]

