## Luganodes SRE task

To do: 
- Design and implement a highly-available Prometheus and Grafana setup with redundant alerts
- Ensure continuous monitoring, data collection and visualization of metrics from various sources, while also guaranteeing redundancy in the alerting system. 
- Alerts should function as a unified system, regardless of the number of Prometheus Mimir, or Loki instances.

Requirements:
- High Availability - used MinIO and a non-authenticated reverse proxy set up using NGINX.
- Redundant alerting - sent Telegram alerts to the following [channel](https://t.me/+yOJpdSe98w5jNjA1) - for two cases, one where the number of servers is being returned continuously, and another severe alert which is fired upon encountering a server that has failed (up == 0 condition)
- Visualization - Dashboards can be created with necessary metrics to visualize them, after setting up data sources via Mimir/Prometheus. Needs tenant ID to query metrics/alerts that are specific to a tenant-ID.
- Fault tolerance - Mimir usually handles faults (?). Tested with 3 Mimir instances as stated in `docker-compose.yml` - with dedicated alert for failure of server, and after one server was killed, the alert was fired. Mimir instance as a whole did not crash; server failure was handled gracefully and no data was [lost](https://github.com/grafana/mimir)
- Monitoring and Alerting - _should be in place to track the health and availability of Prometheus, Mimir and Grafana components._ I couldn't really think of a way to monitor the monitoring tools. We could in theory write a script that checked 
    - if the Docker containers were running; I personally use OrbStack on my Hackintosh to manage containers aside from using the CLI
    - if on a dedicated server, check systemctl status 
    - There is also [this document](https://grafana.com/docs/grafana/latest/setup-grafana/set-up-grafana-monitoring/) - that underlines how to manage Grafana's tracing with Prometheus to manage metrics

To run:
```
$ docker-compose up -d
```

To stop:
```
$ docker-compose down
```

### Endpoints and other config information

- Grafana: https://localhost:9000
- Grafana Mimir: https://localhost:9009
- Prometheus Data Source: https://localhost:9009/prometheus
- X-Scope-OrgID: demo

### References
- Various articles off Google 
- [Getting started with Grafana Mimir](https://grafana.com/docs/mimir/latest/get-started/0)
- [Monitoring a Linux Host with Prometheus, Node Exporter and Docker Compose](https://grafana.com/docs/grafana-cloud/quickstart/docker-compose-linux/)
- [Set up Grafana for high availability](https://grafana.com/docs/grafana/latest/setup-grafana/set-up-for-high-availability/)
- [Play with Grafana Mimir](https://github.com/grafana/mimir/tree/main/docs/sources/mimir/tutorials/play-with-grafana-mimir)