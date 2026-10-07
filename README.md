# NewBank-Infra
Here is a ready-to-use **Overview / Getting Started** section formatted for your repository's [`README.md`](https://github.com/YashMane424/NewBank-Infra/blob/main/README.md):

```markdown
## 🚀 Infrastructure & Development Setup

This project forms the central infrastructure and backend services for **NewBank**.

### Prerequisites

Ensure you have the following installed locally:
- [Docker & Docker Compose](https://www.docker.com/)
- Node.js (for frontend client development)
- Java 17+ / Maven (for backend microservices)

### Environment Configuration

1. Copy the example environment file to create your local `.env`:
   ```bash
   cp .env.example .env

```

2. Update the environment variables inside `.env` as needed for your local setup.

### Local Client & Service Ports

During local development, services communicate across the following ports:

| Service / App | URL / Port | Description |
| --- | --- | --- |
| **Frontend Dev Server (Vite)** | `http://localhost:5173` | Default Vite development server for modern React/Svelte apps. |
| **Frontend Dev Server (CRA/Next)** | `http://localhost:3000` | Default development port for React/Next.js frontend applications. |
| **Auth Service** | Configured in `.env` | Handles JWT authentication, security filters, and CORS permissions for local origin URLs. |

### Running Infrastructure

Start the containerized infrastructure services using Docker Compose:

```bash
docker-compose up -d

```

```

```
