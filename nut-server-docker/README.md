# NUT Server for Raspberry Pi (Dockerized)

This is a self-contained Docker setup for running a Network UPS Tools (NUT) server on a Raspberry Pi, with a USB-connected Eaton UPS.

## Features

- Eaton USB UPS via `usbhid-ups` driver
- Exposes NUT server on port 3493
- Runs standalone with client support
- Easy to deploy via Docker Compose

## Usage

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/nut-server-docker.git
cd nut-server-docker
```

### 2. Build and Run

```bash
docker compose up -d --build
```

### 3. Test UPS

Inside container:

```bash
docker exec -it nut-server upsc eatonups@localhost
```

Remote:

```bash
upsc eatonups@<raspberry-pi-ip>
```

## License

MIT
