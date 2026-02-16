# NOFX Deployment Guide

## Quick Deployment

### One-Click Install (Linux/macOS)

```bash
curl -fsSL https://raw.githubusercontent.com/NoFxAiOS/nofx/main/install.sh | bash
```

After installation, visit: **http://127.0.0.1:3000**

### Docker Compose

```bash
# Download and start
curl -O https://raw.githubusercontent.com/NoFxAiOS/nofx/main/docker-compose.prod.yml
docker compose -f docker-compose.prod.yml up -d

# Management commands
docker compose -f docker-compose.prod.yml logs -f      # View logs
docker compose -f docker-compose.prod.yml restart      # Restart
docker compose -f docker-compose.prod.yml down         # Stop
docker compose -f docker-compose.prod.yml pull && docker compose -f docker-compose.prod.yml up -d  # Update
```

### Railway One-Click Cloud Deployment

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/nofx)

## Windows Installation

### Method 1: Docker Desktop (Recommended)

1. Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
2. Start Docker Desktop
3. Run in PowerShell:
```powershell
curl -o docker-compose.prod.yml https://raw.githubusercontent.com/NoFxAiOS/nofx/main/docker-compose.prod.yml
docker compose -f docker-compose.prod.yml up -d
```

### Method 2: WSL2

1. Install WSL2:
```powershell
wsl --install
```

2. Install Ubuntu, then run the one-click installation script in WSL2

## Manual Installation (Developers)

### Prerequisites

- Go 1.21+
- Node.js 18+
- TA-Lib

```bash
# macOS
brew install ta-lib

# Ubuntu/Debian
sudo apt-get install libta-lib0-dev
```

### Installation Steps

```bash
# 1. Clone repository
git clone https://github.com/NoFxAiOS/nofx.git
cd nofx

# 2. Install backend dependencies
go mod download

# 3. Install frontend dependencies
cd web && npm install && cd ..

# 4. Build and start backend
go build -o nofx && ./nofx

# 5. Start frontend (new terminal)
cd web && npm run dev
```

## Server Deployment

### Quick Deployment (HTTP)

Transport encryption disabled by default, accessible directly via IP:

```bash
curl -fsSL https://raw.githubusercontent.com/NoFxAiOS/nofx/main/install.sh | bash
# Access: http://YOUR_SERVER_IP:3000
```

### HTTPS (Cloudflare)

1. Add domain to Cloudflare
2. Create DNS A record pointing to server IP, enable proxy (orange cloud)
3. Set SSL/TLS to Flexible
4. Edit `.env` to set `TRANSPORT_ENCRYPTION=true`
5. Access: `https://nofx.yourdomain.com`

## Updates

Run daily to get the latest version:

```bash
curl -fsSL https://raw.githubusercontent.com/NoFxAiOS/nofx/main/install.sh | bash
```

## Initial Configuration

1. **Configure AI Models** - Add API Keys
2. **Configure Exchanges** - Set up exchange APIs
3. **Create Strategies** - Configure in Strategy Studio
4. **Create Traders** - Combine AI + Exchange + Strategy
5. **Start Trading** - Launch Traders
