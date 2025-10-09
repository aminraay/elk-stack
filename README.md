# ELK Stack Docker Deployment

Production-ready ELK Stack (Elasticsearch, Logstash, Kibana) with Docker Compose and centralized configuration management.

## Prerequisites

* Docker Engine 20.10+
* Docker Compose 2.0+
* Minimum 4GB RAM available

## Quick Start

Clone and run:

```bash
git clone <repository-url>
cd elk-stack
docker-compose up -d
```

## Access Points

| Service       | URL                   | Port             |
| ------------- | --------------------- | ---------------- |
| Elasticsearch | http://localhost:9200 | 9200             |
| Kibana        | http://localhost:5601 | 5601             |
| Logstash      | -                     | 5044, 5000, 9600 |

## Project Structure

```
elk-stack/
├── docker-compose.yml
├── elasticsearch/
│   └── config/
│       └── elasticsearch.yml
├── logstash/
│   ├── config/
│   │   └── logstash.yml
│   └── pipeline/
│       └── logstash.conf
└── kibana/
    └── config/
        └── kibana.yml
```

## Configuration

All services use external configuration files:

* **Elasticsearch** : `elasticsearch/config/elasticsearch.yml`
* **Logstash** : `logstash/config/logstash.yml` + `logstash/pipeline/logstash.conf`
* **Kibana** : `kibana/config/kibana.yml`

Modify these files and restart services to apply changes.

## Commands

Start stack:

```bash
docker-compose up -d
```

Stop stack:

```bash
docker-compose down
```

View logs:

```bash
docker-compose logs -f
```

Restart service:

```bash
docker-compose restart <service-name>
```

## Health Check

Verify Elasticsearch is running:

```bash
curl http://localhost:9200/_cluster/health
```

## Data Persistence

Elasticsearch data is persisted in Docker volume `elasticsearch_data`.

## Security Note

This configuration has security features disabled for development purposes. For production use, enable xpack security and configure proper authentication.

## License

MIT
