# 🔌 NUT Server Docker for Raspberry Pi

A complete Dockerized setup to run a **Network UPS Tools (NUT)** server on a **Raspberry Pi** with a **USB-connected Eaton UPS**.

---

## ✅ Features

- 🐳 Docker container based on Debian Slim
- 🔌 USB UPS support using `usbhid-ups` driver (Eaton, CyberPower, etc.)
- 📡 Exposes UPS status via NUT on port `3493`
- 🔁 Auto-restarts with Docker
- 🔧 Configurable via simple text files
- 🧩 Compatible with Home Assistant, Synology, Linux NUT clients

---

## 🛠️ 1. Clone the Repository

```bash
git clone https://github.com/barnard704344/nut-server-docker.git
cd nut-server-docker
🚀 2. Build and Run the Container
bash
Copy
Edit
docker compose up -d --build
That's it! Your NUT server will now:

✅ Detect the USB-connected UPS

✅ Start the NUT driver and services

✅ Expose UPS info to other clients on port 3493

✅ Log UPS activity and battery status continuously

🔧 Configuration Files
All configuration is stored in the nut/ directory and bind-mounted into the container's /etc/nut.

📄 nut.conf
conf
Copy
Edit
MODE=standalone
Specifies the NUT mode: standalone, netclient, or netserver

Use standalone if this Pi is connected directly to the UPS

📄 ups.conf
conf
Copy
Edit
[eatonups]
  driver = usbhid-ups
  port = auto
  desc = "Eaton UPS"
  vendorid = "0463"
Declares the UPS device

driver should be usbhid-ups for USB-connected Eaton

port = auto will scan USB devices

vendorid is optional; use lsusb to find the actual ID

📄 upsd.conf
c
Copy
Edit
LISTEN 0.0.0.0 3493
Tells the server to accept incoming client connections on port 3493

Default NUT port; required for LAN clients (Home Assistant, Synology, etc.)

📄 upsd.users
conf
Copy
Edit
[nutmon]
  password = secret
  upsmon master
Defines a user named nutmon with role master

Used by local and remote clients to authenticate

⚠️ Change the password before deploying to production

📄 upsmon.conf
c
Copy
Edit
MONITOR eatonups@localhost 1 nutmon secret master
SHUTDOWNCMD "/sbin/shutdown -h +0"
MINSUPPLIES 1
MONITOR connects to the UPS as user nutmon

Tells NUT to shut down if there’s only 1 UPS (MINSUPPLIES)

SHUTDOWNCMD runs the shutdown command when batteries are critical

🧪 Example Usage
✅ Check UPS status
bash
Copy
Edit
docker exec -it nut-server upsc eatonups@localhost
📜 View logs
bash
Copy
Edit
docker logs -f nut-server
🔌 Connect From Remote Client
Any other device on your network can connect:

bash
Copy
Edit
upsc eatonups@<raspberry_pi_ip>
Replace <raspberry_pi_ip> with the IP of your Pi on the LAN.

🏠 Home Assistant Example Config
yaml
Copy
Edit
sensor:
  - platform: nut
    name: Eaton UPS
    host: 192.168.1.100
    username: nutmon
    password: secret
    resources:
      - battery.charge
      - input.voltage
      - ups.load
      - ups.status
Update the host to your Pi's IP

Change username/password to match upsd.users

🛠️ Advanced Tips
🔐 Change all default credentials in upsd.users

🧪 Test USB detection with lsusb on the Pi

📄 Add logging via volume mount (/var/log/nut)

🧩 Use netclient mode on other devices to monitor this server

🧾 License
MIT License

👨‍💻 Maintainer
Rob Barnard
https://github.com/barnard704344

Open an issue if you'd like support or improvements.

yaml
Copy
Edit

---

✅ You can now copy-paste this entire block into your GitHub `README.md` — it's 100% comp
