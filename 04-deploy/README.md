# VM Application Deployments (Nginx & Apache) - Study & Reference Manual

Welcome to the **VM Application Deployments Guide**. This workspace folder contains a collection of comprehensive, deep-dive manuals designed to take you from a basic system setup to deploying highly available web applications on Virtual Machines using Nginx and Apache.

## Course Structure & Document Map

Below is the structured layout of the course documents. Each file contains detailed explanations, diagrams, step-by-step commands, configurations, real-world examples, and troubleshooting guides.

### 🗺️ Course Map

1. **[01. Nginx Fundamentals](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/01-nginx-fundamentals.md)**
   - Introduction to Nginx (Event-driven architecture)
   - Installation & Status Management
   - Basic Config File blocks (`http`, `server`, `location`)
2. **[02. Nginx Reverse Proxy & Load Balancer](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/02-nginx-reverse-proxy.md)**
   - What is a Reverse Proxy? (Direct vs. Reverse Proxy)
   - Configuring Nginx as a Reverse Proxy
   - Load Balancing Upstreams (Round Robin, Least Connections, IP Hash)
3. **[03. Apache Fundamentals](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/03-apache-fundamentals.md)**
   - Introduction to Apache (Process-based architecture)
   - Installation & Config layouts
   - Setting up Virtual Hosts & `.htaccess` configurations
4. **[04. Deploying Static Applications](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/04-deploying-static-apps.md)**
   - Deploying HTML/CSS/React Build static websites to Nginx and Apache default locations
   - Permissions adjustments (`www-data` ownerships)
5. **[05. Deploying Dynamic Applications](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/05-deploying-dynamic-apps.md)**
   - Deploying Node.js applications with PM2 process manager
   - Deploying Python WSGI applications with Gunicorn
   - Mapping Nginx reverse proxy configuration to local backend ports
6. **[06. Hands-on Labs](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/06-hands-on-labs.md)**
   - Lab 1: Deploy a static portfolio site using Nginx Virtual Hosts
   - Lab 2: Configure a Load Balancer to distribute traffic between two local backend web applications
7. **[07. Troubleshooting & Logs](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/07-troubleshooting-and-logs.md)**
   - Locate and examine server log paths
   - Diagnose and resolve common HTTP status codes (403 Permission denied, 404 Not Found, 502 Bad Gateway, 504 Gateway Timeout)
8. **[08. Interview Prep & Viva Bank](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/08-interview-questions-and-viva.md)**
   - Frequently asked server management and DevOps deploy interview questions
9. **[Server Management Cheat Sheet](file:///c:/Users/Mehebub_Alam/Desktop/Projects/DevOps-Doc/04-deploy/server-cheat-sheet.md)**
   - Command quick card and template config blocks for Nginx and Apache
