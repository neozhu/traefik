# Home Lab Development Environment with Traefik, DDNS, and Docker

This project demonstrates how to set up a development environment for a home lab using Traefik, DDNS, and Docker. It provides a complete solution to manage services in a containerized environment with automatic DNS resolution and reverse proxy configuration.


## Prerequisites

- Docker and Docker Compose installed
- Traefik configured as a reverse proxy
- DDNS service to handle dynamic DNS
- Basic knowledge of Docker, Traefik, and DNS management

To better understand and guide you through the setup of the entire environment, please refer to these videos:

- [Video 1: Traefik v3.3 - Secure Everything! Complete Tutorial](https://www.youtube.com/watch?v=CmUzMi5QLzI&list=PL77ZBY9FxekDLlyv6m6mQbgzacOlFYnm8&index=2)
- [Video 2: Simple HTTPs for Docker! // Traefik Tutorial (updated)](https://www.youtube.com/watch?v=-hfejNXqOzA&list=PL77ZBY9FxekDLlyv6m6mQbgzacOlFYnm8&index=2)


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
sudo chmod 600 acme.json
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

## 9 Enroll Security Engine
enroll security engine by execing into the crowdsec container
```bash
docker exec crowdsec cscli console enroll -e context <YOUR-CODE>
```

## 10 Creating a Bouncer API Token
After approving our security engine, we have to create a Traefik bouncer for our CrowdSec container. The bouncer requires an API token, which will be created in the process.

To add a bouncer, run the following command:
```bash
docker exec crowdsec cscli bouncers add traefik-bouncer
```
Please make note of this API token, as we have to define it later on within a Traefik middleware configuration.

## Testing Manual IP Banning
To ban or unban an IP address manually in CrowdSec, you can use the following CSCLI commands via Docker exec:
```bash
# manually ban an ip address
docker exec crowdsec cscli decisions add --ip <IP>

# manually unban an ip address
docker exec crowdsec cscli decisions remove --ip <IP>
```

## Troubleshooting
- If you cannot access the Traefik dashboard, check if the password was set up correctly.
- If services are not being routed, ensure that the labels and rules in your Docker Compose file are correctly defined for Traefik.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

