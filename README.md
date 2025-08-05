# TheHive & Cortex Docker Stack

> Quick Docker setup for TheHive and Cortex platforms for security incident management and automated analysis.

## 🚀 Quick Start

```bash
git clone <your-repo>
cd thehive-cortex-stack
docker-compose up -d
```

**Access URLs:**
- TheHive: http://localhost:9000
- Cortex: http://localhost:9001

## 📋 What's Included

This stack includes the following services:

| Service | Port | Description |
|---------|------|-------------|
| TheHive | 9000 | Security Incident Response Platform |
| Cortex | 9001 | Automated Analysis Engine |
| Elasticsearch | 9200 | Data storage for TheHive |
| Cassandra | 9042 | Data storage for Cortex |

## ⚙️ Prerequisites

- Docker & Docker Compose
- Minimum 4GB RAM
- 10GB free disk space

## 🛠️ Installation Steps

### 1. Clone Repository
```bash
git clone <repository-url>
cd thehive-cortex-stack
```

### 2. System Configuration (Linux)
```bash
sudo sysctl -w vm.max_map_count=262144
```

### 3. Start Services
```bash
docker-compose up -d
```

### 4. Initial Setup
1. Go to TheHive: http://localhost:9000
2. Create admin account
3. Go to Cortex: http://localhost:9001  
4. Create superadmin account

### 5. Integration
1. Create API key in Cortex
2. Replace `YOUR_CORTEX_API_KEY` in `thehive/application.conf`
3. Restart TheHive: `docker-compose restart thehive`

## 📊 Health Checks

```bash
# Service status
docker-compose ps

# Follow logs
docker-compose logs -f

# API health check
curl http://localhost:9000/api/status
curl http://localhost:9001/api/status
```

## 🔧 Common Issues

### Elasticsearch won't start
```bash
sudo sysctl -w vm.max_map_count=262144
docker-compose restart elasticsearch
```

### Cassandra connection error
```bash
# Wait 30-60 seconds, then check
docker-compose logs cassandra
```

### TheHive database error
```bash
curl -X DELETE "localhost:9200/thehive"
docker-compose restart thehive
```

## 🔒 Security

⚠️ **Must change for production:**
- Secret keys in `application.conf` files
- Default passwords
- Network configurations

## 📁 Directory Structure

```
thehive-cortex-stack/
├── docker-compose.yml
├── thehive/
│   └── application.conf
├── cortex/
│   └── application.conf
└── README.md
```

## 🔄 Updates

```bash
docker-compose pull
docker-compose up -d
```

## 💾 Backup

```bash
# Backup Elasticsearch
docker run --rm -v thehive-cortex-stack_elasticsearch_data:/data \
  -v $(pwd):/backup alpine tar czf /backup/es_backup.tar.gz -C /data .

# Backup Cassandra  
docker run --rm -v thehive-cortex-stack_cassandra_data:/data \
  -v $(pwd):/backup alpine tar czf /backup/cassandra_backup.tar.gz -C /data .
```

## 📚 Documentation

- [TheHive Documentation](https://docs.thehive-project.org/)
- [Cortex Documentation](https://github.com/TheHive-Project/Cortex)
- [Docker Compose Guide](https://docs.docker.com/compose/)

## 🆘 Support

If you encounter issues:
1. Check logs: `docker-compose logs`
2. Review GitHub Issues
3. Contact TheHive Community

## 🌟 Features

- **One-command deployment**
- **Pre-configured integration** between TheHive and Cortex
- **Persistent data storage** with Docker volumes
- **Production-ready** configuration templates
- **Comprehensive troubleshooting** guide

## 🔧 Customization

### Adding Analyzers
1. Access Cortex web interface
2. Go to Organization settings
3. Enable analyzer repositories
4. Install required analyzers

### TheHive Configuration
- Modify case templates in the web interface
- Configure webhooks for notifications
- Set up LDAP authentication (edit `thehive/application.conf`)

### Performance Tuning
```bash
# Increase memory allocation
export COMPOSE_DOCKER_CLI_BUILD=1
export DOCKER_BUILDKIT=1

# Edit docker-compose.yml JVM_OPTS:
# - JVM_OPTS="-Xms2048m -Xmx2048m"
```

## 🚦 Service Management

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

## 📈 Monitoring

```bash
# Resource usage
docker stats

# Service health
curl -s http://localhost:9000/api/status | jq
curl -s http://localhost:9001/api/status | jq

# Database connectivity
docker-compose exec elasticsearch curl -X GET "localhost:9200/_cluster/health"
docker-compose exec cassandra cqlsh -e "SELECT cluster_name FROM system.local;"
```

## 🏗️ Architecture

```
┌─────────────┐    ┌─────────────┐
│   TheHive   │◄──►│   Cortex    │
│   :9000     │    │   :9001     │
└─────────────┘    └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐    ┌─────────────┐
│Elasticsearch│    │  Cassandra  │
│   :9200     │    │   :9042     │
└─────────────┘    └─────────────┘
```

---

**Note:** This setup is suitable for development and testing environments. For production deployment, implement additional security measures and consider high availability configurations.