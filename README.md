# VPS Stack Configuration

This repository contains the Docker Compose configuration for a production VPS stack with Traefik, CrowdSec, Loki, and monitoring.

## 🔐 Secrets Management

Sensitive values are stored in local files that are excluded from git. Example files with placeholders are provided.

### Setup Required Files

1. **`.env`** - Main environment variables
   ```bash
   # WUD (What's Up Docker) Configuration
   WUD_DISCORD_WEBHOOK=https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN

   # CrowdSec Configuration
   CROWDSEC_BOUNCER_API_KEY=your_bouncer_key
   CROWDSEC_LAPI_KEY=your_lapi_key

   # Tailscale Addresses
   HOST_TAILSCALE_IP=100.x.x.x
   NEBULA_TAILSCALE_ADDRESS=http://nebula.tailxxxxx.ts.net
   ```

2. **`cloudflare.env`** - Cloudflare API token for DNS challenge
   ```
   your_cloudflare_api_token_here
   ```

3. **`http-discord.yaml`** - CrowdSec Discord webhook notification config
   ```bash
   cp http-discord.yaml.example http-discord.yaml
   # Edit and replace YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN with actual values
   ```

4. **`crowdsec/whitelist-home.yaml`** - Home IP whitelist
   ```bash
   cp crowdsec/whitelist-home.yaml.example crowdsec/whitelist-home.yaml
   # Edit and replace placeholder IPs with your actual home IPs
   ```

## 🚀 Quick Start

1. Copy example files and configure with your actual values:
   ```bash
   cp http-discord.yaml.example http-discord.yaml
   cp crowdsec/whitelist-home.yaml.example crowdsec/whitelist-home.yaml

   # Edit each file with your actual secrets
   nano http-discord.yaml
   nano crowdsec/whitelist-home.yaml

   # Create .env and cloudflare.env files
   nano .env
   nano cloudflare.env
   ```

2. Start the stack:
   ```bash
   docker compose up -d
   ```

## 📁 File Structure

- `*.example` - Template files with placeholders (safe to commit)
- `*.yaml` / `*.yml` - Configuration files
- `.env` - Secret environment variables (gitignored)
- `cloudflare.env` - Cloudflare API token (gitignored)

### Traefik Configuration

Traefik supports `{{ env "VAR" }}` syntax in dynamic configs, so variables can be referenced directly from environment variables passed via Docker Compose.

### CrowdSec Configuration

CrowdSec does not support template syntax, so configs with secrets use the `.example` pattern:
- `http-discord.yaml` - Discord webhook notification (has `.example` version)
- `profiles-discord.yaml` - Notification profiles (has `.example` version)
- `crowdsec/whitelist-home.yaml` - Home IP whitelissafe to commit, no secretsion)
- `crowdsec/http-probing-custom.yaml` - Custom HTTP probing scenario
- `crowdsec/http-crawl-non_statics-custom.yaml` - Custom crawl detection scenario

## 🔒 What's Safe to Commit?

✅ **Safe to commit:**
- All `*.example` files with placeholders
- Configuration files without secrets
- Docker Compose files
- Scripts and documentation

❌ **Never commit:**
- `.env` - Contains all environment secrets
- `cloudflare.env` - Contains API token
- `http-discord.yaml` - Contains Discord webhook URL
- `crowdsec/whitelist-home.yaml` - Contains your home IP addresses
- `traefik-logs/` - Access logs with IPs
- `letsencrypt/` - SSL certificates
- `crowdsec/*.db` - CrowdSec databases
- `wud/store/` - Runtime data

See `.gitignore` for the complete list of excluded files.
