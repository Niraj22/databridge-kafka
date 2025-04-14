# DataBridge Kafka Messaging Infrastructure

This repository contains the Docker Compose configuration for the DataBridge project's Kafka infrastructure, which serves as the messaging backbone for the event-driven microservices architecture.

## Components

- **Zookeeper**: Coordination service for Kafka
- **Kafka**: Distributed event streaming platform
- **Kafka UI**: Web interface for managing and monitoring Kafka
- **Kafka Init**: Service to initialize Kafka topics on startup

## Prerequisites

- Docker and Docker Compose installed on your system
- At least 2GB of free RAM for running the services
- Open ports: 2181, 9092, 29092, and 8080

## Setup and Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/your-org/databridge-kafka.git
   cd databridge-kafka
   ```

2. Create the Docker Compose file:
   ```bash
   # Save the configuration as docker-compose-kafka.yml
   nano docker-compose-kafka.yml
   # Copy and paste the configuration provided
   ```

## Starting the Services

Start all Kafka infrastructure services:

```bash
docker-compose -f docker-compose-kafka.yml up -d
```

To view logs while starting:

```bash
docker-compose -f docker-compose-kafka.yml up
```

## Verifying the Setup

1. Check that all services are running:
   ```bash
   docker-compose -f docker-compose-kafka.yml ps
   ```

2. Verify Kafka topics were created:
   ```bash
   docker exec databridge-kafka kafka-topics --bootstrap-server localhost:9092 --list
   ```

3. Access Kafka UI in your browser:
   ```
   http://localhost:8080
   ```

## Service Endpoints

- **Zookeeper**: localhost:2181
- **Kafka** (internal): kafka:9092
- **Kafka** (external): localhost:29092
- **Kafka UI**: http://localhost:8080

## Stopping the Services

Stop all services without removing volumes:

```bash
docker-compose -f docker-compose-kafka.yml stop
```

Stop and remove containers, networks, but preserve volumes:

```bash
docker-compose -f docker-compose-kafka.yml down
```

Complete cleanup including volumes:

```bash
docker-compose -f docker-compose-kafka.yml down -v
```

## Restarting Individual Services

Restart just Kafka:

```bash
docker-compose -f docker-compose-kafka.yml restart kafka
```

Recreate topic initialization service:

```bash
docker-compose -f docker-compose-kafka.yml stop kafka-init
docker-compose -f docker-compose-kafka.yml rm -f kafka-init
docker-compose -f docker-compose-kafka.yml up -d kafka-init
```

## Available Kafka Topics

The following topics are automatically created:

- CustomerCreated
- CustomerUpdated
- CustomerPreferenceChanged
- ProductCreated
- ProductUpdated
- InventoryChanged
- InventoryLow
- PriceChanged
- OrderCreated
- OrderStatusChanged
- PaymentProcessed
- ShipmentUpdated
- user.login.success
- user.login.failed

## For Application Configuration

When connecting to Kafka:
- From services running in the same Docker network, use: `kafka:9092`
- From services running on the host or in different networks, use: `localhost:29092`

## Troubleshooting

- **Kafka won't start**: Check if Zookeeper is healthy using `docker logs databridge-zookeeper`
- **Topics not created**: Run the topic creation manually with `docker exec databridge-kafka kafka-topics --bootstrap-server localhost:9092 --create --topic <topic_name> --partitions 3 --replication-factor 1`
- **Connection refused in applications**: Verify whether your application is inside the Docker network or outside, and use the appropriate Kafka address

## Maintenance

- Kafka data is persisted in the `kafka-data` volume
- Zookeeper data is persisted in the `zookeeper-data` and `zookeeper-log` volumes
- To upgrade Kafka, update the image version in the docker-compose file

For further assistance, consult the project's internal documentation or contact the infrastructure team.
