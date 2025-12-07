# Kali-MCP-Warp

podman run -d \
  --name kali \
  --privileged \
  -p 22:22 \
  -p 5000:5000 \
  kalilinux/kali-rolling tail -f /dev/null


podman exec -it kali bash

apt update && apt -y install kali-linux-headless

sudo apt install mcp-kali-server




sudo nano /etc/systemd/system/kali-mcp.service


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


Reload systemd:
sudo systemctl daemon-reload

Enable the service at boot:
sudo systemctl enable kali-mcp

Start the service:
sudo systemctl start kali-mcp.  >>> this will show Running on http://127.0.0.1:5000

Check status:
sudo systemctl status kali-mcp

To view logs:
sudo journalctl -u kali-mcp -f

------------------------------------

Host PC

git clone https://github.com/Wh0am123/MCP-Kali-Server.git  or download the zip file 

remember the  /path/to/MCP-Kali-Server/mcp_server.py


Open Warp >> Setting >> MCP Servers >> click Add for adding new MCP servers 

Paste the config in this format:

{
  "kali-mcp": {
    "args": [
      "/Users/B0024335/sushil/mcp_wrapper/MCP-Kali-Server/mcp_server.py",
      "--server",
      "http://127.0.0.1:8765"
    ],
    "command": "python3.11"
  }
}


Its Done, restart the Warp, you will see 12 tools.


