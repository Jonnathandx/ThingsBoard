# ThingsBoard CE with PostgreSQL - Docker Compose

This repository contains a Docker Compose configuration to deploy ThingsBoard Community Edition (4.1.0) with PostgreSQL (17.5) as the database.

## Prerequisites

- Docker installed
- Docker Compose installed
- SSL certificates (place them in the `./ssl` directory)

## Services

### 1. PostgreSQL Database
- Image: `postgres:17.5`
- Port: 5432 (exposed internally)
- Database name: `thingsboard`
- Password: `postgres`
- Timezone: `America/Guayaquil`
- Data persistence: `./data` directory mounted to container

### 2. ThingsBoard CE
- Image: `thingsboard/tb-node:4.1.0`
- Exposed ports:
  - 8080: HTTP/HTTPS web interface
  - 7070: HTTP API
  - 1883: MQTT
  - 8883: MQTT over SSL
  - 5683-5688: CoAP and other IoT protocols (UDP)
- Timezone: `America/Guayaquil`
- SSL enabled for web interface and MQTT
- Logging configured with rotation (100MB max, 10 files)

## SSL Configuration

Place your SSL certificates in the `./ssl` directory with the following requirements:
- `server.pem`: Combined certificate file (should include certificate chain and private key)

## Getting Started

1. Clone this repository
2. Create an `ssl` directory and place your SSL certificates
3. Run the following command:

```bash
docker-compose up -d
