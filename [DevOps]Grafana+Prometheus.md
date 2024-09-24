Grafana and Prometheus:

1. Download Prometheus and node_exporter
2. Install Prometheus node_exporter
3. Install and configure Prometheus
4. Configure Prometheus for Grafana
5. Check Prometheus metrics in Grafana Explore view
6. Start building dashboards

Download Prometheus and node_exporter

Download the following components:

![Prometheus](https://prometheus.io/download/#prometheus)

![node_exporter](https://prometheus.io/download/#node_exporter)

Like Grafana, you can install Prometheus on many different operating systems. Refer to the Prometheus download page to see a list of stable versions of Prometheus components.

Install Prometheus node_exporter

Install node_exporter on all hosts you want to monitor. This guide shows you how to install it locally.

Prometheus node_exporter is a widely used tool that exposes system metrics. For instructions on installing node_exporter, refer to the Installing and running the node_exporter section in the Prometheus documentation.

When you run node_exporter locally, navigate to http://localhost:9100/metrics to check that it is exporting metrics.
```
Note
The instructions in the referenced topic are intended for Linux users. You may have to alter the instructions slightly depending on your operating system. For example, if you are on Windows, use the windows_exporter instead.
```

