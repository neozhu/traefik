# Home Lab Development Environment with Traefik, DDNS, and Docker

This project demonstrates how to set up a development environment for a home lab using Traefik, DDNS, and Docker. It provides a complete solution to manage services in a containerized environment with automatic DNS resolution and reverse proxy configuration.


## Prerequisites

- Docker and Docker Compose installed
- Traefik configured as a reverse proxy
- DDNS service to handle dynamic DNS
- Basic knowledge of Docker, Traefik, and DNS management

## Setup Instructions


Follow these steps to set up your home lab environment with Traefik, DDNS, and Docker:

## 1. Register a Domain
Sign up for a domain and set up a wildcard subdomain (`*.domain`). We recommend using Cloudflare for DNS management.

## 2. Create an Access Token
Generate an access token on Cloudflare and use it to configure the environment. After obtaining the token, input it into the `cf-token` variable in your configuration.

## 3. Clone the Repository
Clone the repository from GitHub to get started with the setup:

```bash
git clone https://github.com/neozhu/traefik.git
cd traefik
```

## 4. Set Up Traefik Dashboard Password
Create a password for the Traefik dashboard. Install `apache2-utils` if not already installed, then run the following command to generate the password:

```bash
echo $(htpasswd -nB admin) | sed -e s/\\$/\\$\\$/g
```

Once you have the password, update the `TRAFFIK_DASHBOARD_CREDENTIALS` variable in the `.env` file with your generated password.

## 5. Run the Environment
Run the following Docker Compose command to start the containers:

```bash
sudo docker compose up -d
```

## 6. Configure DDNS for Wildcard Subdomain
Set up DDNS for your wildcard subdomain `*.domain`. Access the configuration page at `http://localhost:9876` and follow the instructions to configure DDNS.

## 7. Update Traefik Configuration
Ensure that your Traefik configuration files are correctly set to route traffic to the right containers. This can involve setting up routers and entry points in the Traefik configuration files.

## 8. Access Services
After configuring everything, you can access the services using the domain you set up with DDNS. Traefik will manage the reverse proxy to route traffic to the respective services.

## Troubleshooting
- If you cannot access the Traefik dashboard, check if the password was set up correctly.
- If services are not being routed, ensure that the labels and rules in your Docker Compose file are correctly defined for Traefik.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

