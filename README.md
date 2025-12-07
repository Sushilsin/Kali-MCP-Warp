Deploying Kali Linux + MCP Tools API Server in Podman

A Complete Guide for Setting Up Kali MCP Server and Connecting It with Warp/Clients

Overview

This document provides step-by-step instructions for:
	1.	Creating a Kali Linux container using Podman
	2.	Installing kali-linux-headless and MCP Kali Tools Server
	3.	Running MCP as a systemd service inside the container
	4.	Connecting the host machine to the MCP Server (e.g., Warp, LM Studio, CLI)

⸻

1. Create Kali Container in Podman

Run the following command on your host system:

podman run -d \
  --name kali \
  --privileged \
  -p 22:22 \
  -p 5000:5000 \
  kalilinux/kali-rolling tail -f /dev/null

Access the container:

podman exec -it kali bash


⸻

2. Install Kali Packages + MCP Server

Update and install standard Kali CLI tools:

apt update && apt -y install kali-linux-headless

Install the Kali MCP Tools Server:

sudo apt install mcp-kali-server


⸻

3. Create systemd Service for MCP Server

Create the service file:

sudo nano /etc/systemd/system/kali-mcp.service

Paste the following:

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

# Environment variables (optional)
Environment="API_PORT=5000"
Environment="DEBUG_MODE=0"

# Security Hardening
NoNewPrivileges=true
ProtectSystem=full
ProtectHome=true

# Log output
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target


⸻

4. Enable and Start the MCP Server

Reload systemd:

sudo systemctl daemon-reload

Enable MCP at boot:

sudo systemctl enable kali-mcp

Start MCP service:

sudo systemctl start kali-mcp

Check status:

sudo systemctl status kali-mcp

Expected output:

Running on http://127.0.0.1:5000

View logs:

sudo journalctl -u kali-mcp -f


⸻

Host PC Configuration

5. Download the MCP Client Wrapper

Clone the repository:

git clone https://github.com/Wh0am123/MCP-Kali-Server.git

Or download the ZIP from GitHub.

Important file path to note:

/path/to/MCP-Kali-Server/mcp_server.py


⸻

6. Add MCP Server in Warp

Open:

Warp → Settings → MCP Servers → Add Server

Paste the following configuration:

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

Restart Warp.

After restart, you will see 12 MCP tools available (nmap, gobuster, dirb, nikto, sqlmap, etc.).

⸻

Summary

You now have:
	•	A Kali Linux Podman container running MCP Tools API Server
	•	A systemd-managed MCP service running on port 5000
	•	Warp/LM-Studio/Ollama-based AI clients connecting to your Kali tools
	•	12 full-function security tools exposed via MCP interface

This setup enables AI-driven offensive security, automated scanning, and seamless integration with any MCP-capable LLM client.
