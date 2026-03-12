# 🔐 Kali MCP Tools API Server  
## (Podman + tmux + Codex Integration)

A containerized Kali Linux environment exposing security tooling via an MCP-compatible API, designed for controlled AI-agent interaction (Codex / LLM integrations).

---

## 📌 Architecture Overview

Codex / AI Agent
↓
MCP Client (Host)
↓ HTTP (127.0.0.1:5000)
Kali MCP Server (Podman Container)
↓
Kali Security Tools

---

# 🚀 Setup Guide

---

## 1️⃣ Run Kali Container (Podman)

```bash
podman run -dit \
  --name kali \
  --hostname kali-mcp \
  -p 5000:5000 \
  --cap-add=NET_RAW \
  --cap-add=NET_ADMIN \
  kalilinux/kali-rolling

Enter container:

podman exec -it kali bash


⸻

2️⃣ Install Required Packages (Inside Container)

Update system:

apt update
apt full-upgrade -y

Install required tools:

apt install -y kali-linux-headless python3 python3-pip tmux mcp-kali-server


⸻

🖥 3️⃣ Run MCP Server Inside tmux

Running the server inside tmux ensures:
	•	Process continues after terminal disconnect
	•	Stable background execution
	•	Easy re-attachment for monitoring

⸻

Start tmux session

tmux new -s mcp


⸻

Run MCP server inside tmux

kali-server-mcp --ip 0.0.0.0 --port 5000


⸻

Detach from tmux (server keeps running)

Press:

CTRL + B, then D


⸻

Reattach to tmux session later

tmux attach -t mcp


⸻

List active tmux sessions

tmux ls


⸻

4️⃣ Verify Server is Listening

Inside container:

ss -tulnp | grep 5000

Expected output:

0.0.0.0:5000

From host:

curl http://127.0.0.1:5000


⸻

🖥 5️⃣ Configure MCP Client (Host)

Clone repository:

git clone https://github.com/Wh0am123/MCP-Kali-Server.git
cd MCP-Kali-Server

Create virtual environment:

python3.11 -m venv mcp-env
source mcp-env/bin/activate
pip install -r requirements.txt

Test connectivity:

curl http://127.0.0.1:5000


⸻

⚙ 6️⃣ Configure Codex (config.toml)

Edit:

[mcp_servers.kali-mcp]
enabled = true
command = "python3.11"
args = [
  "/absolute/path/MCP-Kali-Server/mcp_server.py",
  "--server",
  "http://127.0.0.1:5000"
]

Replace with actual absolute path:

pwd


⸻

🛡 Security Considerations

⚠️ This project exposes offensive security tooling via API. Use only in authorized lab environments.

Recommended Hardening
	•	Avoid --privileged
	•	Bind to 127.0.0.1 in production
	•	Add:
	•	API authentication
	•	Command allowlist
	•	Rate limiting
	•	Audit logging
	•	Restrict container network
	•	Apply firewall controls

⸻

🔎 Troubleshooting

Check container port mapping:

podman port kali

Check MCP process:

ps aux | grep kali-server-mcp

Check tmux session:

tmux ls


⸻

⚖️ Disclaimer

For:
	•	Security research
	•	Educational use
	•	Authorized penetration testing labs only

Unauthorized use against systems without explicit permission is illegal.

