# Local AI Config
Docker-Compose for setting up some local AI services using ollama for code complete and open-webui for testing models.

## Setup

To setup these services you need:
1. Domains or subdomains for web service and llm-provider
2. cloudflare.ini with Cloudflare dns api token to enable certbot to 
3. Tailscale API access token from the Tailscale OAuth client 

## Services
### tailscale-ai
- **Purpose**: Manages network connectivity using Tailscale to access AI models remotely.
- **Configuration**:
  - Need to pass in the Tailscale API access token by setting SECRET_TS_AUTHKEY

### certbot
- **Purpose**: Manages SSL certificates using Certbot with Cloudflare DNS API.
- **Configuration**:
  - Need to mount the cloudflare.ini secret configurations into container
  - Need to pass in domains for web and llm-provider to allow https access through tailscale

### nginx
- **Purpose**: Provides reverse proxy to AI services and HTTPs termination using certs from certbot. 
- **Configuration**:
  - Need to environment variables setup domains used for accessing the web ui.
  - Template for sites is in ./nginx/templates
  - Network connected through Tailscale container to allow remote access through tailscale.

### llm-provider
- **Purpose**: Provides language model services currently using Ollama (but might change to vLLM in the future).
- **Configuration**:
  - This is configured to use the ROCm ollama since I am running on an AMD GPU

### web-ui
- **Purpose**: Provides a web-based interface using Open Web UI.
- **Configuration**:
  - Needs to provide the docker url (hostname + internal port) for the llm-provider
