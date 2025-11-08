# KitchenOwl Docker Setup

This repository contains a Docker Compose setup for running KitchenOwl, a smart grocery list and recipe manager.

## Prerequisites

- Docker and Docker Compose installed
- An external Docker network named `scoobydoo` (or modify the compose.yaml to use a different network)

## Setup Instructions

### 1. Create the External Network

If you haven't already created the `scoobydoo` network, run:

```bash
docker network create scoobydoo
```

### 2. Configure Environment Variables

Copy the example environment file and configure it:

```bash
cp .env.example .env
```

Edit the `.env` file and set the following variables:

- `JWT_SECRET_KEY`: A secure random secret key for JWT token signing (see generation instructions below)
- `HTTP_PORT`: The port on which KitchenOwl will be accessible (default: 8080)

#### Generating a Secure JWT Secret Key

To generate a secure random secret key, use OpenSSL:

```bash
openssl rand -hex 64
```

Copy the output and paste it as the value for `JWT_SECRET_KEY` in your `.env` file.

### 3. Start the Application

Launch KitchenOwl using Docker Compose:

```bash
docker compose up -d
```

### 4. Access the Application

Once running, access KitchenOwl at:

```
http://localhost:8040
```

(Replace `8040` with your configured `HTTP_PORT` value)

## Configuration

### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `JWT_SECRET_KEY` | Secret key for JWT token signing | Yes | None |
| `HTTP_PORT` | Host port to expose the application | No | 8080 |
| `FRONT_URL` | Public URL for accessing KitchenOwl (used for links and CORS) | No | None |

### Volumes

- `kitchenowl_data`: Persistent storage for application data

### Networks

- `scoobydoo`: External network for inter-container communication

## Maintenance

### View Logs

```bash
docker compose logs -f
```

### Stop the Application

```bash
docker compose down
```

### Update to Latest Version

```bash
docker compose pull
docker compose up -d
```

### Backup Data

The application data is stored in the `kitchenowl_data` volume. To backup:

```bash
docker run --rm -v kitchenowl_data:/data -v $(pwd):/backup alpine tar czf /backup/kitchenowl-backup.tar.gz /data
```

### Restore Data

```bash
docker run --rm -v kitchenowl_data:/data -v $(pwd):/backup alpine tar xzf /backup/kitchenowl-backup.tar.gz -C /
```

## Troubleshooting

### Container won't start

Check the logs for errors:
```bash
docker compose logs
```

### JWT_SECRET_KEY error

Ensure your `.env` file contains a valid `JWT_SECRET_KEY`. Generate a new one if needed:
```bash
openssl rand -hex 64
```

## More Information

For more information about KitchenOwl, visit:
- GitHub: https://github.com/TomBursch/kitchenowl
- Documentation: https://docs.kitchenowl.org
