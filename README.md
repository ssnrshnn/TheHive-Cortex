# TheHive & Cortex Docker Stack

## Quick Start

```bash
git clone https://github.com/ssnrshnn/TheHive-Cortex
cd thehive-cortex-stack
docker-compose up -d
```

**Access URLs:**
- TheHive: http://localhost:9000
- Cortex: http://localhost:9001

## What's Included

This stack includes the following services:

| Service | Port | Description |
|---------|------|-------------|
| TheHive | 9000 | Security Incident Response Platform |
| Cortex | 9001 | Automated Analysis Engine |
| Elasticsearch | 9200 | Data storage for TheHive |
| Cassandra | 9042 | Data storage for Cortex |

### Initial Setup
1. Go to TheHive: http://localhost:9000
2. Create admin account
3. Go to Cortex: http://localhost:9001  
4. Create superadmin account

### Integration
1. Create API key in Cortex
2. Replace `YOUR_CORTEX_API_KEY` in `thehive/application.conf`
3. Restart TheHive: `docker-compose restart thehive`

## Health Checks

```bash
# Service status
docker-compose ps

# Follow logs
docker-compose logs -f

# API health check
curl http://localhost:9000/api/status
curl http://localhost:9001/api/status
```

## Cortex Analyzers and Responders

```bash
# Clone analyzers (in your project directory)
cd /home/thehive
git clone https://github.com/TheHive-Project/Cortex-Analyzers.git

# Fix permissions
sudo chown -R 1000:1000 Cortex-Analyzers/

# Restart
docker-compose down
docker-compose up -d
```








## Service Management

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# Restart specific service
docker-compose restart thehive

# View service logs
docker-compose logs -f cortex

# Remove everything (including data)
docker-compose down -v
```

## Security

⚠️ **Must change for production:**
- Secret keys in `application.conf` files
- Default passwords
- Network configurations

## Directory Structure

```
thehive-cortex-stack/
├── docker-compose.yml
├── thehive/
│   └── application.conf
├── cortex/
│   └── application.conf
└── README.md
```

## 📚 Documentation

- [TheHive Documentation](https://docs.thehive-project.org/)
- [Cortex Documentation](https://github.com/TheHive-Project/Cortex)
- [Docker Compose Guide](https://docs.docker.com/compose/)
