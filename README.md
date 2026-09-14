# 🌐 Personal Digital Ecosystem (PDE)

## 🌟 Overview
The Personal Digital Ecosystem (PDE) is a comprehensive, self-hosted infrastructure designed to manage, automate, and serve all personal digital requirements. Built entirely on **Docker** and **Docker Compose**, this ecosystem acts as a cohesive digital nervous system, bridging the gap between disparate services (like media streaming, data logging, and AI inference) into a unified, highly accessible environment.

This project champions the principle of **centralized control and modular design**, allowing services to scale and adapt independently while presenting a singular, secure gateway to the user.

---

## 🔬 Key Features & Technical Highlights
*   **Modular Deployment:** Services are isolated into distinct functional domains (sectors) using dedicated `docker-compose.yml` files.
*   **Zero-Trust Access:** All incoming traffic is funneled through **Traefik**, enforcing mandatory HTTPS redirection and providing a unified entry point.
*   **Centralized Authentication:** Security is managed by an integrated **Authentik** layer, ensuring all accessed services require authentication via a single provider.
*   **Containerized Resilience:** Services are deployed as immutable containers managed by Docker, ensuring consistent environments across development and production.
*   **Data Persistence:** Configuration is managed via a central `.env` file, while all persistent data is organized in a designated `$datafolder` structure (`${datafolder}/${container_name}`).

---

## 🗺️ Architectural Blueprint
The PDE operates on a secure, segmented network structure. All services are allocated specific, non-overlapping CIDR blocks, enforcing network segmentation for optimal security and routing.

### 🖥️ Networking & Routing
*   **Gateway:** Traefik acts as the ingress router, handling DNS resolution, SSL termination (via Let's Encrypt integration), and load balancing.
*   **Authentication Layer:** Authentik is the security authority, validating tokens and authorizing access before requests reach the respective services.
*   **Traffic Flow:** `External Request` $\rightarrow$ `Traefik` $\rightarrow$ (Auth Check via `Authentik`) $\rightarrow$ `Target Sector Service`

### 🗄️ Data Persistence
All containers map their persistent storage to a central directory defined in the `.env` file. This organization facilitates simple backups and migration.

---

## 🛠️ System Segmentation (Sectors)
Each domain is managed by its own dedicated set of `docker-compose` files, enabling granular control over deployment and scaling.

| Sector | Primary Function | Key Technologies | Example CIDR Range |
| :--- | :--- | :--- | :--- |
| **AI** | Model Inference & Processing | Custom Code, ML Frameworks | `10.0.0.0/24` |
| **Database** | Data Storage & Querying | PostgreSQL, MySQL, etc. | `10.0.1.0/24` |
| **Automation** | Home Control & Events | Home Assistant, Zigbee2MQTT, MQTT | `10.0.2.0/24` |
| **Multimedia** | Media Serving & Indexing | Immich, Jellyfin | `10.0.3.0/24` |
| **Monitoring** | Observability & Metrics | Prometheus, Grafana | `10.0.4.0/24` |
| **Networking** | Core Infrastructure | Router/Edge Services, AdGuard | `10.0.5.0/24` |

---

## 🚀 Getting Started

### Prerequisites
Ensure you have the following installed on your host machine:
*   Docker Engine
*   Docker Compose (or Docker Compose V2)

### Configuration Steps
1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/mouradost/the-nexus-ecosystem.git
    cd the-nexus-ecosystem
    ```
2.  **Configure Environment:**
    Copy the sample file to create your personal configuration:
    ```bash
    cp .env.example .env
    ```
    Edit the `.env` file to define critical parameters:
    *   `DATA_FOLDER`: Set the absolute path where all persistent volumes will live (e.g., `/mnt/pve/pde_data`).
    *   *Update any specific host names or external service credentials.*
3.  **Deployment:**
    To deploy the entire ecosystem, run the main stack command:
    ```bash
    # Deploy all services defined in the root docker-compose.yml
    docker compose up -d
    ```
    *Alternatively, you can deploy sectors individually:*
    ```bash
    # Deploy only the Automation and Multimedia stacks
    docker compose -f docker-compose-automation.yml -f docker-compose-multimedia.yml up -d
    ```

### Accessing Services
All services are accessible via HTTPS at your domain. Accessing a specific service is done via its designated sub-domain (or path), routed by Traefik:

*   **API Gateway:** `https://pde.yourdomain.com`
*   **Automation:** `https://home.yourdomain.com` (Routing to Home Assistant)
*   **Media:** `https://media.yourdomain.com` (Routing to Immich/Jellyfin)
*   **Monitoring:** `https://monitor.yourdomain.com` (Routing to Grafana)

---

## 📚 Tech Stack Summary
*   **Core Platform:** Docker, Docker Compose
*   **Orchestration/Proxy:** Traefik
*   **Authentication:** Authentik
*   **Primary Language/Engine:** Rust (Used in custom containers)
*   **Configuration:** Shell Scripts, `.env` Files
*   **Persistence Layer:** Volume Mapping (within `$datafolder`)
