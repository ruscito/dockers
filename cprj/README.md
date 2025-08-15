# C Development Docker Image with Vim

This Docker image provides a ready-to-use C development environment with **Vim** and a custom `.vimrc`. It includes build tools, a root and non-root user, and persistent workspaces.

---

## Features

* **C development tools**:

  * `gcc`, `g++`, `make`, `build-essential`
* **Vim editor** with your custom `.vimrc` loaded for the dev user
* **Root and non-root users** (`dev` user is default, root available via `sudo`)
* **Persistent workspace**:

  * `/workspace` is mapped to your host folder
  * `/home/dev` is a named volume to persist your Vim config and shell history
* Works on **Linux**, **macOS**, and **Windows PowerShell**

---

## Requirements

* Docker (20.10+)
* Docker Compose (1.29+)

---

## Setup

1. Clone or copy the repository containing the `Dockerfile` and `docker-compose.yml`.
2. Ensure your `.vimrc` is configured in the Dockerfile, or update the `VIMRC_URL` environment variable to your Gist raw URL.

---

## Build the Image

```bash
docker compose build
```

This will build the image `c-vim-dev` with C build tools and Vim installed.

---

## Run the Container

### Linux / macOS

```bash
docker compose run --rm -it c-vim-dev
```

### Windows PowerShell

```powershell
docker compose run --rm -it c-vim-dev
```

* The container will start with an interactive shell as `dev`.
* Your project files in the host folder are accessible in `/workspace` inside the container.
* Use Vim to edit files or GCC/Make to build projects inside the container.

---

## Accessing Root

If you need root privileges inside the container:

```bash
sudo su -
```

---

## Persisted Data

* Your **project files** in `/workspace` remain on your host machine.
* Your **dev user home** (`/home/dev`) is stored in a Docker volume `dev-home`, so Vim configuration and shell history are retained between container runs.

---

## Notes

* The `.vimrc` is preloaded from your specified Gist URL. If you want to modify it, update the file in the container or bake a new image.
* You can compile and run C programs from the terminal using `gcc`, `g++`, and `make`.

---

## License

This setup is provided for personal or educational use and is based on open-source software.

