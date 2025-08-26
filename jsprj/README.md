# JavaScript / React / React Native Development with Docker

This Docker setup provides a ready-to-use environment for learning and building **JavaScript**, **React**, and **React Native** projects.  
It includes Node.js, npm, Yarn, Expo CLI, React Native CLI, Watchman, and JDK for Android build tooling.

---

## Features

* **JavaScript / React tools**:
  * Node.js (LTS), npm, Yarn (via Corepack)
  * React Native CLI
  * Expo CLI
  * Watchman (for file watching in React Native)
  * OpenJDK 17 (for Android builds)
* **Dev-friendly environment**:
  * Non-root `dev` user with sudo access
  * Vim editor included
* **Persistent workspace**:
  * `/workspace` → mapped to your host folder
  * `/home/dev` → Docker volume to persist configs, history, and npm cache
* **Port forwarding for development**:
  * `3000` → React web apps
  * `8081` → Metro bundler
  * `19000–19006` → Expo development ports

---

## Requirements

* Docker (20.10+)
* Docker Compose (1.29+)

---

## Setup

1. Clone or copy the repository containing the `Dockerfile` and `docker-compose.yml`.
2. Create a local `workspace/` folder next to the Docker files. This will hold your projects.

---

## Build the Image

```bash
docker compose build
docker compose run --rm i=-it js-dev
```
