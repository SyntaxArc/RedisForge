# RedisForge

RedisForge is a production-ready Redis deployment using Docker Compose, designed for reliability, persistence, and ease of use. This project leverages the lightweight `redis:7.4.2-alpine` image to provide a robust single-node Redis instance.

## Project Structure
```
redisforge/
├── single-node/
│   ├── conf/
│   │   └── redis.conf    # Redis configuration file
│   ├── docker-compose.yml # Docker Compose configuration
│   └── .env              # Environment variables (e.g., password)
└── README.md             # This file
```

## Features
- **Lightweight Base**: Uses `redis:7.4.2-alpine` for minimal resource usage.
- **Data Persistence**: Configured with both AOF and RDB for durability.
- **Security**: Password protection via environment variables.
- **Reliability**: Automatic container restarts with `restart: always`.
- **Modular Design**: Configuration separated into `conf/redis.conf`.

## Configuration Details
- **Redis Version**: `7.4.2-alpine`.
- **Persistence**: Data stored in a named volume `redis-data`.
- **Configuration File**: `single-node/conf/redis.conf` with production-ready settings.
- **Security**: Password set in `single-node/.env`.

### redis.conf Settings
```
appendonly yes           # Enables Append-Only File persistence
appendfsync everysec     # Fsync every second for durability
save 60 1000             # RDB snapshot every 60s if 1000 keys change
maxmemory 1024mb         # Limits memory to 1GB
maxmemory_policy allkeys-lru # Evicts least recently used keys
```

## Prerequisites
- Docker and Docker Compose installed on your system.

## Setup Instructions
1. **Clone or Create the Structure**:
   Ensure all files are in the `redisforge` directory as shown above.

2. **Configure Password**:
   Edit `single-node/.env` and replace the default value:
   ```
   REDIS_PASSWORD=mysecretpassword
   ```
   Use a strong, secure password.

3. **Start RedisForge**:
   Navigate to `single-node` and run:
   ```bash
   cd single-node
   docker compose up -d
   ```
   This launches Redis in detached mode.

4. **Verify Deployment**:
   - Check container status: `docker ps`
   - Test connection: `docker exec -it redis redis-cli -h localhost -p 6379 -a <your_password>`

## Stopping the Service
From `single-node`:
```bash
docker compose down
```
To remove the volume and reset data:
```bash
docker compose down -v
```

## Usage
- Connect to Redis using any client (e.g., `redis-cli`, libraries like `ioredis` or `redis-py`).
- Default port is `6379`, accessible locally or via mapped host port.

## Customization
- Adjust `single-node/conf/redis.conf` for your workload (e.g., tweak `maxmemory` or `save` settings).
- Modify `single-node/docker-compose.yml` to expose additional ports or add environment variables.

## Notes
- The named volume `redis-data` ensures data persists across container restarts.
- For high availability, consider extending this setup with Redis replication or clustering.
- Backup the `redis-data` volume regularly for critical deployments.

## License
This project has **No License**. You are free to use it as you see fit, but there are no warranties or permissions explicitly granted.
