# Monitoring Docker



## Usage

1.- Configuration

1.1.- Add your domains to ./prometheus_config/prometheus.yml

```yml
...
          - http://example.com    # Change this to your domain.
          - https://example.com   # Don't forget https! :)
...
```
1.2 docker-compose.yml configurations

```yml
  promtail:
    image: grafana/promtail:master
    volumes:
      - /var/log/nginx:/var/log # Path where your logs are located
      - ./promtail_config:/etc/promtail/ # path to log job location
    command: -config.file=/etc/promtail/docker-config.yaml
    networks:
      - monitoring
    restart: always

  grafana:
    image: grafana/grafana
    ports:
      - 3000:3000
    environment:
      - GF_SERVER_ROOT_URL=<base_url> # your grafana url
      - GF_SECURITY_ADMIN_PASSWORD=<PASSWORD> # your grafana admin password
    volumes:
      - "./grafana_data:/var/lib/grafana"
    networks:
      - monitoring
    restart: always