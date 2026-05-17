# Nginx Reverse Proxy

This service provides a central reverse proxy using **Nginx**.  
It acts as the single entry point for all external traffic coming into the homelab.  
Multiple external DNS names are forwarded to this Nginx instance, and Nginx routes each request to the correct internal container based on the hostname.

---

## What This Nginx Setup Does

- Accepts incoming HTTPS traffic from the internet  
- Uses TLS certificates issued by the ACME service  
- Matches the request's DNS name (Server Name Indication)  
- Routes the request to the correct backend container  
- Provides a single, secure, centralised gateway for all services  

This allows you to expose multiple services through one public IP address while keeping everything neatly organised behind Nginx.

---

## How Traffic Flows

1. A user visits one of your domain names.  
2. Your router forwards ports 80 and 443 to the Nginx container.  
3. Nginx receives the request and checks the hostname.  
4. Nginx selects the correct server block for that hostname.  
5. The request is proxied to the matching internal container.  

Each service has its own server block inside the Nginx configuration.

---

## Adding a New Service

To expose a new container through Nginx:

1. Ensure the domain name points to your public IP.  
2. Add a new certificate entry in your ACME configuration so the domain has valid TLS.  
3. Create a new server block in your Nginx configuration.  
4. Point the proxy settings to the internal container's hostname and port.  
5. Reload or restart the Nginx container.

Once this is done, Nginx will automatically route traffic for the new domain to the correct backend.

---

## Notes

- Only ports 80 and 443 need to be exposed externally.  
- All backend containers remain internal and unreachable from the internet.  
- Each domain name gets its own server block for clarity and maintainability.  
- Certificates are renewed automatically by the ACME service and mounted into Nginx.  
- Nginx does not need to run as root inside the container.  

This setup keeps your homelab secure, organised, and easy to expand as you add more services.


