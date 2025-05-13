# Selenium Grid 4 - Docker Compose Setup

This repository contains a ready-to-run **Selenium Grid 4 setup using Docker Compose**, supporting **multiple Chrome, Firefox, and Edge nodes**. You can choose to run only the desired browser node by using the respective configuration files.

---

## 📦 Components

| Service      | Image                         | Description                         |
| ------------ | ----------------------------- | ----------------------------------- |
| selenium-hub | selenium/hub\:latest          | Central Grid Hub (UI, API, routing) |
| chrome       | selenium/node-chrome\:latest  | Chrome Node (with VNC support)      |
| firefox      | selenium/node-firefox\:latest | Firefox Node (with VNC support)     |
| edge         | selenium/node-edge\:latest    | Edge Node (with VNC support)        |

---

## 🖥 Features

* ✔ Selenium Grid 4 fully distributed architecture.
* ✔ Multiple Chrome, Firefox, and Edge nodes for parallel test execution.
* ✔ Pre-configured Event Bus (`4442`, `4443`) communication.
* ✔ VNC support for browser session debugging (`VNC_PASSWORD=12345`).
* ✔ Docker network isolation (`selenium-grid`).
* ✔ Easily extendable for more nodes or additional browsers.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Kartik240803/selenium-grid-docker-setup.git
cd selenium-grid-docker-setup
```

### 2. Start the Grid

To start the grid with only **Chrome** node:

```bash
docker-compose -f docker-compose-chrome.yml up -d
```

To start the grid with only **Firefox** node:

```bash
docker-compose -f docker-compose-firefox.yml up -d
```

To start the grid with only **Edge** node:

```bash
docker-compose -f docker-compose-edge.yml up -d
```

### 3. Access Grid UI

* 📍 [http://localhost:4444](http://localhost:4444)

---

## 🔧 Adding More Nodes (Optional)

You can easily add **more Chrome**, **Firefox**, or **Edge** nodes to the grid by modifying the respective compose file.

### Scaling Chrome Nodes

If you want to add more **Chrome** nodes, you can scale them using the following command (from the `docker-compose-chrome.yml` file):

```bash
docker-compose -f docker-compose-chrome.yml up -d --scale chrome=5
```

> ⚠ Ensure you rename your Chrome service to `chrome` instead of `chrome1`, `chrome2`, etc.

### Scaling Firefox Nodes

To add more **Firefox** nodes (from the `docker-compose-firefox.yml` file), use:

```bash
docker-compose -f docker-compose-firefox.yml up -d --scale firefox=5
```

### Scaling Edge Nodes

To add more **Edge** nodes (from the `docker-compose-edge.yml` file), use:

```bash
docker-compose -f docker-compose-edge.yml up -d --scale edge=5
```

> ⚠ Make sure to scale nodes by adjusting the `--scale` argument to your needs.

---

## 🔧 How to Add a New Node to Any Compose File

You can add more nodes (Chrome, Firefox, or Edge) to any of the following `docker-compose` files:

* **For Chrome Node**: Add the configuration to `docker-compose-chrome.yml`.
* **For Firefox Node**: Add the configuration to `docker-compose-firefox.yml`.
* **For Edge Node**: Add the configuration to `docker-compose-edge.yml`.

Example for adding a **Chrome Node** to any compose file:

```yaml
chrome:
  image: selenium/node-chrome:latest
  shm_size: 2gb
  depends_on:
    - selenium-hub
  environment:
    - SE_EVENT_BUS_HOST=selenium-hub
    - SE_EVENT_BUS_PUBLISH_PORT=4442
    - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
    - VNC_PASSWORD=12345
  networks:
    - selenium-grid
```

Similarly, you can add **Firefox** or **Edge** nodes following the same approach.

After modifying the `docker-compose` file, you can restart the grid with:

```bash
docker-compose up -d
```

---

## ⚙ Configuration Details

### Docker Compose Structure for **Chrome** Node (`docker-compose-chrome.yml`)

```yaml
services:
  selenium-hub:
    image: selenium/hub:latest
    ports:
      - "4444:4444"
      - "4442:4442"
      - "4443:4443"

  chrome:
    image: selenium/node-chrome:latest
    shm_size: 2gb
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - VNC_PASSWORD=12345
```

### Docker Compose Structure for **Firefox** Node (`docker-compose-firefox.yml`)

```yaml
services:
  selenium-hub:
    image: selenium/hub:latest
    ports:
      - "4444:4444"
      - "4442:4442"
      - "4443:4443"

  firefox:
    image: selenium/node-firefox:latest
    shm_size: 2gb
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - VNC_PASSWORD=12345
```

### Docker Compose Structure for **Edge** Node (`docker-compose-edge.yml`)

```yaml
services:
  selenium-hub:
    image: selenium/hub:latest
    ports:
      - "4444:4444"
      - "4442:4442"
      - "4443:4443"

  edge:
    image: selenium/node-edge:latest
    shm_size: 2gb
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - VNC_PASSWORD=12345
```

---

### Exposed Ports

| Port | Description                              |
| ---- | ---------------------------------------- |
| 4444 | Grid UI and API                          |
| 4442 | Event Bus Publish                        |
| 4443 | Event Bus Subscribe                      |
| 5900 | VNC for browser nodes (inside container) |

---

## 🛡 Best Practices

* Always use the `latest` tag to ensure you are using the most up-to-date version.
* Use Docker networks (`bridge`) for isolation.
* Prefer **scaling** for multiple browser nodes rather than hardcoding duplicate services.

---

## 🔗 References

* [Selenium Official Docker Hub](https://hub.docker.com/u/selenium)
* [Selenium Grid 4 Documentation](https://www.selenium.dev/documentation/grid/)
* [Docker Compose Docs](https://docs.docker.com/compose/)

---

## 📄 License

MIT License.
