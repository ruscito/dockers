# Deep Learning with Python Docker Image

This Docker image provides a ready-to-use development environment for **"Deep Learning with Python"** by François Chollet. It includes Python, TensorFlow/Keras, JupyterLab, and a Vim setup, all pre-configured for deep learning experiments.

---

## Features

* **Python 3.11** with essential scientific libraries:

  * `tensorflow`, `keras`
  * `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `seaborn`, `plotly`
* **JupyterLab** for interactive notebooks
* **Vim editor** with your custom `.vimrc` (loaded for the dev user)
* **Root and non-root users** (`dev` user is default, root available via `sudo`)
* **Persistent workspace**:

  * `/workspace` is mapped to your host folder
  * `/home/dev` is a named volume to persist your Vim config and shell history
* Works on **Linux**, **macOS**, and **Windows PowerShell**

---

## Requirements

* Docker (20.10+)
* Docker Compose (1.29+)

Optional for GPU support:

* NVIDIA drivers + `nvidia-docker2` (not included by default)

---

## Setup

1. Clone or copy the repository containing the `Dockerfile` and `docker-compose.yml`.
2. Ensure your `.vimrc` is configured in the Dockerfile, or update the `VIMRC_URL` environment variable to your Gist raw URL.

---

## Build the Image

```bash
docker compose build
```

This will build the image `deep-learning` with Python, JupyterLab, and deep learning libraries installed.

---

## Run the Container

### Linux / macOS

```bash
docker compose run --rm -p 8888:8888 deep-learning
```

### Windows PowerShell

```powershell
docker compose run --rm -p 8888:8888 deep-learning
```

* JupyterLab will start inside the container and bind to port `8888`.
* Open your browser and navigate to the URL provided in the console, e.g., `http://localhost:8888/?token=...`.
* Your project files in the host folder are accessible in `/workspace` inside the container.

### Use Jupyter and terminal simultaneously
```bash
docker compose up -d
docker exec -it deep-learning bash
```

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
* The environment is CPU-only by default. For GPU support, you will need to use an NVIDIA base image and configure `--gpus` in Docker Compose.

---

## License

This setup is provided for personal or educational use and is based on open-source software.
