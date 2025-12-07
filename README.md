
<p align="center">
  <img src="https://img.shields.io/badge/Kali%20MCP%20Server%20%7C%20Podman-blue?logo=kalilinux&style=for-the-badge" />
</p>

<h1 align="center">Kali MCP Tools API Server in Podman</h1>

<p align="center">
  Run Kali Linux + MCP Security Tools inside Podman and integrate with Warp, LM Studio, and Ollama.
</p>


⸻

2. Full README.md with Badges (GitHub-Optimized)

Below is a fully formatted, production-quality README.md.

⸻


<p align="center">
  <img src="https://img.shields.io/badge/Kali%20MCP%20Server%20%7C%20Podman-blue?logo=kalilinux&style=for-the-badge" />
</p>

<h1 align="center">Kali MCP Tools API Server in Podman</h1>

<p align="center">
  Deploy Kali Linux inside Podman and expose 12+ offensive security tools over the Model Context Protocol (MCP).
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Podman-Enabled-purple?style=flat-square&logo=podman" />
  <img src="https://img.shields.io/badge/Kali%20Linux-Rolling-blue?style=flat-square&logo=kalilinux" />
  <img src="https://img.shields.io/badge/MCP-Server-green?style=flat-square" />
  <img src="https://img.shields.io/badge/Warp-Compatible-orange?style=flat-square&logo=warp" />
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square" />
</p>

---

## 🚀 Features

- Full **Kali headless environment** inside Podman  
- MCP Server exposing 12+ tools including:
  - nmap  
  - gobuster  
  - dirb  
  - nikto  
  - sqlmap  
  - ffuf (optional)
- Integrates seamlessly with:
  - Warp Terminal  
  - LM Studio  
  - Ollama  
  - Custom MCP clients
- Managed as a **systemd service** inside container  
- API served on `0.0.0.0:5000`

---

## 🛠️ 1. Create the Kali Podman Container

```bash
podman run -d \
  --name kali \
  --privileged \
  -p 22:22 \
  -p 5000:5000 \
  kalilinux/kali-rolling tail -f /dev/null

Enter the container:

podman exec -it kali bash


⸻

🗂️ 2. Install Kali Packages & MCP Server

apt update && apt -y install kali-linux-headless
sudo apt install mcp-kali-server


⸻

🔧 3. Configure MCP as a systemd Service

Create service file:

sudo nano /etc/systemd/system/kali-mcp.service

Add:

[Unit]
Description=Kali MCP Tools API Server
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/usr/share/mcp-kali-server
ExecStart=/usr/bin/python3 /usr/share/mcp-kali-server/kali_server.py --ip 0.0.0.0 --port 5000
Restart=always
RestartSec=5

Environment="API_PORT=5000"
Environment="DEBUG_MODE=0"

NoNewPrivileges=true
ProtectSystem=full
ProtectHome=true

StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target

Enable service:

sudo systemctl daemon-reload
sudo systemctl enable kali-mcp
sudo systemctl start kali-mcp

Check:

sudo systemctl status kali-mcp

Logs:

sudo journalctl -u kali-mcp -f


⸻

💻 4. Configure MCP on Host Machine (Warp Example)

Clone MCP client repo:

git clone https://github.com/Wh0am123/MCP-Kali-Server.git

Warp → Settings → MCP Servers → Add

{
  "kali-mcp": {
    "args": [
      "/path/to/MCP-Kali-Server/mcp_server.py",
      "--server",
      "http://127.0.0.1:5000"
    ],
    "command": "python3.11"
  }
}

Restart Warp → You should now see 12 MCP Tools.

⸻

🧩 Architecture Diagram

                         ┌───────────────────────────────────┐
                         │     Host Machine (macOS/Linux)     │
                         │-------------------------------------│
                         │ Warp / LM Studio / Ollama / MCP CLI │
                         └──────────────────┬──────────────────┘
                                            │  MCP Protocol
                                            │  HTTP (5000)
                                            ▼
                   ┌───────────────────────────────────────────────┐
                   │            Podman Container (Kali)             │
                   │------------------------------------------------│
                   │  Kali Linux Rolling                             │
                   │  MCP Tools API Server (systemd)                 │
                   │  Exposed Tools: nmap, dirb, gobuster, nikto…    │
                   │  Port 5000 → MCP HTTP Interface                 │
                   └───────────────────┬─────────────────────────────┘
                                       │
                                       │ OS/Container Boundary
                                       ▼
                   ┌───────────────────────────────────────────────┐
                   │                 Security Tools                 │
                   │------------------------------------------------│
                   │ /usr/bin/nmap                                 │
                   │ /usr/bin/sqlmap                               │
                   │ /usr/bin/dirb                                 │
                   │ /usr/bin/gobuster                             │
                   │ /usr/bin/nikto                                │
                   └───────────────────────────────────────────────┘


⸻

📜 License

MIT License. Free for personal and commercial use.

⸻

🤝 Contributing

Pull requests are welcome.
Please open an issue for discussions and enhancements.

⸻

⭐ Support the Project

If this repo helped you, consider starring it on GitHub.

---

# **3. Delivery Summary**

You now have:

✔ GitHub-style **banner**  
✔ Full **README.md** with badges  
✔ A clean **ASCII architecture diagram**  
✔ Everything ready for GitHub publishing

---

If you want, I can also add:

- A **Mermaid diagram** (GitHub renders it natively)  
- A **project logo** (PNG/SVG)  
- A **Podmanfile / Dockerfile** for automated builds  
- A **demo GIF** showing Warp using MCP tools  

Would you like these added?
