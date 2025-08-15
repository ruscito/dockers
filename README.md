# Development Docker Projects

This repository contains two separate Docker-based development environments:

1. **C Development with Vim**
2. **Deep Learning with Python**

Each project is self-contained, providing a ready-to-use development setup with persistent workspaces, pre-installed tools, and user configuration.

---

## Projects Overview

### 1. C Development with Vim

* Pre-installed **C compiler tools** (`gcc`, `g++`, `make`) and **build-essential** packages
* **Vim editor** with custom `.vimrc` preloaded
* Supports both root and non-root users (`dev` user by default)
* Persistent workspace in `/workspace` and home folder `/home/dev`
* Detailed README available in `c-project/README.md`

### 2. Deep Learning with Python

* Python 3.11 with essential scientific libraries (`tensorflow`, `keras`, `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `seaborn`, `plotly`)
* **JupyterLab** for notebooks
* **Vim editor** with custom `.vimrc`
* Supports both root and non-root users (`dev` user by default)
* Persistent workspace in `/workspace` and home folder `/home/dev`
* Detailed README available in `deep-learning/README.md`

---

## Common Features

* Docker Compose-based setup for cross-platform compatibility (Linux, macOS, Windows PowerShell)
* Persistent workspace and user home
* Interactive shell support for editing scripts, compiling C programs, or running Python scripts
* Root access available via `sudo`

---

## Getting Started

### Build Images

Navigate to the respective project folder and run:

```bash
docker compose build
```

### Run Containers

#### Linux / macOS

```bash
docker compose run --rm -it <project-name>
```

#### Windows PowerShell

```powershell
docker compose run --rm -it <project-name>
```

Replace `<project-name>` with `c-vim-dev` or `deep-learning`.

---

## Folder Structure

```
├── c-project/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── README.md
├── deep-learning/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── README.md
└── README.md  # This root README
```

---

## License

Each project is provided for personal or educational use and based on open-source software.

