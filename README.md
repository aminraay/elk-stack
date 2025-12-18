# ELK Stack with Docker Compose

A production-ready Elasticsearch, Logstash, and Kibana stack with automated SSL certificate generation and secure authentication.

## Getting Started

Clone the repository to your local machine.

```bash
git clone git@github.com:aminraay/elk-stack.git
cd elk-stack
```

Create the certificates directory that will hold SSL certificates.

```bash
mkdir certs
```

Copy the environment template and configure your credentials.

```bash
cp .env.example .env
```

Edit the `.env` file and replace the placeholder passwords with strong, unique values. The encryption key can remain as is or be changed to a random 32-character string.

```
ELASTIC_USERNAME=elastic
ELASTIC_PASSWORD=your_strong_password_here
KIBANA_USERNAME=kibana_system
KIBANA_SYSTEM_PASSWORD=your_kibana_password_here
LOGSTASH_SYSTEM_PASSWORD=your_logstash_password_here
KIBANA_ENCRYPTION_KEY=JK9p6Lm7xF8zQ2wT6ve6Yu1bn4cX0dS3aE5gH4kP
```

Start the stack with Docker Compose.

```bash
docker-compose up -d
```

The first run will automatically generate SSL certificates and configure user passwords. This process may take a few minutes.

## Accessing the Services

Once the stack is running, you can access the services at the following addresses.

Kibana is available at `http://localhost:5601`. Log in with the elastic username and the password you set in the `.env` file.

Elasticsearch API is available at `https://localhost:9200`. You can verify it's running with curl.

```bash
curl -k -u elastic:your_password https://localhost:9200
```

Logstash is listening on port 5000 for TCP and UDP traffic, with the monitoring API on port 9600.

## Adding New Logstash Pipelines

To add a new pipeline, create a configuration file in the `configs/logstash/pipeline` directory.

```bash
touch configs/logstash/pipeline/your-pipeline.conf
```

Register the pipeline in `configs/logstash/pipelines.yml` by adding a new entry.

```yaml
- pipeline.id: your-pipeline
  path.config: "/usr/share/logstash/pipeline/your-pipeline.conf"
```

Update the docker-compose.yml to mount your new pipeline configuration.

```yaml
- ./configs/logstash/pipeline/your-pipeline.conf:/usr/share/logstash/pipeline/your-pipeline.conf:ro
```

Restart Logstash to apply the changes.

```bash
docker-compose restart logstash
```

## Certificate Management

SSL certificates are generated automatically on first run. They are stored in the `certs` directory and include a Certificate Authority along with certificates for Elasticsearch and Kibana.

If you need to regenerate certificates, remove the contents of the `certs` directory and restart the stack.

```bash
rm -rf certs/*
docker-compose up -d
```

## Stopping the Stack

To stop all services, run the following command.

```bash
docker-compose down
```

To stop and remove all data including Elasticsearch indices, add the volumes flag.

```bash
docker-compose down -v
```

## Project Structure

The configuration files for each service are located in the `configs` directory. Elasticsearch settings are in `configs/elasticsearch/elasticsearch.yml`, Kibana settings in `configs/kibana/kibana.yml`, and Logstash configuration includes `logstash.yml`, `pipelines.yml`, and individual pipeline files in the `pipeline` subdirectory.

SSL certificates are stored in the `certs` directory after generation.

## Troubleshooting

If services fail to start, check the logs for specific errors.

```bash
docker-compose logs <service-name>
```

For Elasticsearch connection issues, verify that certificates were generated correctly by checking the `certs` directory for the presence of `.crt` and `.key` files.

If authentication fails, ensure the passwords in your `.env` file match and that the setup-users service completed successfully.
