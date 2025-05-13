# Selenium Grid 4 - Docker Compose Setup

This repository contains a ready-to-run **Selenium Grid 4 setup using Docker Compose**, supporting **multiple Chrome nodes**, with the option to extend it to **Firefox, Edge, or other browsers**.

---

## 📦 Components

| Service      | Image                                | Description                         |
| ------------ | ------------------------------------ | ----------------------------------- |
| selenium-hub | selenium/hub:4.21.0-20240424         | Central Grid Hub (UI, API, routing) |
| chrome1      | selenium/node-chrome:4.21.0-20240424 | Chrome Node (with VNC support)      |
| chrome2      | selenium/node-chrome:4.21.0-20240424 | Chrome Node (with VNC support)      |
| chrome3      | selenium/node-chrome:4.21.0-20240424 | Chrome Node (with VNC support)      |

---

## 🖥 Features

* ✔ Selenium Grid 4 fully distributed architecture.
* ✔ Multiple Chrome nodes for parallel test execution.
* ✔ Pre-configured Event Bus (`4442`, `4443`) communication.
* ✔ VNC support for browser session debugging (`VNC_PASSWORD=12345`).
* ✔ Docker network isolation (`selenium-grid`).
* ✔ Easily extendable for Firefox and Edge nodes.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-repo>/selenium-grid-docker-compose.git
cd selenium-grid-docker-compose
```

### 2. Start the Grid

```bash
docker-compose up -d
```

### 3. Access Grid UI

* 📍 [http://localhost:4444](http://localhost:4444)

### 4. Scale Chrome Nodes (Optional)

You can dynamically scale Chrome nodes using:

```bash
docker-compose up -d --scale chrome=5
```

> ⚠ Ensure you rename your Chrome service to `chrome` instead of `chrome1`, `chrome2`, etc.
> See scaling-ready example in the config section below.

---

## 🔧 Adding Firefox or Edge Nodes (Optional)

You can easily add **Firefox or Edge nodes** to the grid by adding the following services to your `docker-compose.yml`:

### Add Firefox Node

```yaml
firefox:
  image: selenium/node-firefox:4.21.0-20240424
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

### Add Edge Node

```yaml
edge:
  image: selenium/node-edge:4.21.0-20240424
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

> 🔗 You can access running browser sessions via VNC on port `5900` inside the node container. Use the password `12345`.

### 🛠 Restart the Grid

```bash
docker-compose up -d
```

Now, your Grid UI will show **Chrome, Firefox, and Edge nodes** ready for test execution.

---

## ⚙ Configuration Details

### Docker Compose Structure

```yaml
services:
  selenium-hub:
    image: selenium/hub:4.21.0-20240424
    ports:
      - "4444:4444"
      - "4442:4442"
      - "4443:4443"

  chrome:
    image: selenium/node-chrome:4.21.0-20240424
    shm_size: 2gb
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - VNC_PASSWORD=12345
```

### Exposed Ports

| Port | Description                              |
| ---- | ---------------------------------------- |
| 4444 | Grid UI and API                          |
| 4442 | Event Bus Publish                        |
| 4443 | Event Bus Subscribe                      |
| 5900 | VNC for browser nodes (inside container) |

---

## 🛡 Best Practices

* Always use specific image versions like `4.21.0-20240424` to avoid accidental updates.
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
