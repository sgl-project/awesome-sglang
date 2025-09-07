---
description: Observability
---

# Metrics

## Setup Guide

This section describes how to set up the monitoring stack (Prometheus + Grafana) provided in the `examples/monitoring` directory.

### Prerequisites

* Docker and Docker Compose installed
* SGLang server running with metrics enabled

### Usage

*   **Start your SGLang server with metrics enabled:**

    {% code overflow="wrap" %}
    ```
    python -m sglang.launch_server --model-path <your_model_path> --port 30000 --enable-metrics
    ```
    {% endcode %}

    Replace `<your_model_path>` with the actual path to your model (e.g., `meta-llama/Meta-Llama-3.1-8B-Instruct`). Ensure the server is accessible from the monitoring stack (you might need `--host 0.0.0.0` if running in Docker). By default, the metrics endpoint will be available at `http://<sglang_server_host>:30000/metrics`.
*   **Navigate to the monitoring example directory:**

    ```
    cd examples/monitoring
    ```
*   **Start the monitoring stack:**

    ```
    docker compose up -d
    ```

    This command will start Prometheus and Grafana in the background.
* **Access the monitoring interfaces:**
  * **Grafana:** Open your web browser and go to [http://localhost:3000](http://localhost:3000/).
  * **Prometheus:** Open your web browser and go to [http://localhost:9090](http://localhost:9090/).
* **Log in to Grafana:**
  * Default Username: `admin`
  * Default Password: `admin` You will be prompted to change the password upon your first login.
* **View the Dashboard:** The SGLang dashboard is pre-configured and should be available automatically. Navigate to `Dashboards` -> `Browse` -> `SGLang Monitoring` folder -> `SGLang Dashboard`.
* **Check if the metrics are being collected**:&#x20;
  * Run `python3 -m sglang.bench_serving --backend sglang --dataset-name random --num-prompts 3000 --random-input 1024 --random-output 1024 --random-range-ratio 0.5` to generate some requests.
  * Then you should be able to see the metrics in the Grafana dashboard.

## Available Metrics

### Token Processing

* prompt\_tokens\_total - Counter tracking total prefill tokens processed
* generation\_tokens\_total - Counter tracking total generation tokens processed
* token\_usage - Gauge showing current token usage percentage
* cache\_hit\_rate - Gauge showing cache hit rate percentage

### Performance

* gen\_throughput - Gauge showing generation throughput in tokens/second
* num\_running\_reqs - Gauge showing number of currently running requests
* num\_queue\_reqs - Gauge showing number of queued requests
* num\_used\_tokens - Gauge showing number of tokens currently in use

### Latency Histograms

* time\_to\_first\_token\_seconds - Histogram of time to first token latency
* e2e\_request\_latency\_seconds - Histogram of end-to-end request latency
* time\_per\_output\_token\_seconds - Histogram of time per output token
* func\_latency\_seconds - Histogram of function execution latency

## Troubleshooting

* **Port Conflicts:** If you encounter errors like “port is already allocated,” check if other services (including previous instances of Prometheus/Grafana) are using ports `9090` or `3000`. Use `docker ps` to find running containers and `docker stop <container_id>` to stop them, or use `lsof -i :<port>` to find other processes using the ports. You might need to adjust the ports in the `docker-compose.yaml` file if they permanently conflict with other essential services on your system.

To modify Grafana’s port to the other one(like 3090) in your Docker Compose file, you need to explicitly specify the port mapping under the grafana service.

````
Option 1: Add GF_SERVER_HTTP_PORT to the environment section:
```
  environment:
- GF_AUTH_ANONYMOUS_ENABLED=true
- GF_SERVER_HTTP_PORT=3090  # <-- Add this line
```
Option 2: Use port mapping:
```
grafana:
  image: grafana/grafana:latest
  container_name: grafana
  ports:
  - "3090:3000"  # <-- Host:Container port mapping
```
````

* **Connection Issues:**
  * Ensure both Prometheus and Grafana containers are running (`docker ps`).
  * Verify the Prometheus data source configuration in Grafana (usually auto-configured via `grafana/datasources/datasource.yaml`). Go to `Connections` -> `Data sources` -> `Prometheus`. The URL should point to the Prometheus service (e.g., `http://prometheus:9090`).
  * Confirm that your SGLang server is running and the metrics endpoint (`http://<sglang_server_host>:30000/metrics`) is accessible _from the Prometheus container_. If SGLang is running on your host machine and Prometheus is in Docker, use `host.docker.internal` (on Docker Desktop) or your machine’s network IP instead of `localhost` in the `prometheus.yaml` scrape configuration.
* **No Data on Dashboard:**
  *   Generate some traffic to your SGLang server to produce metrics. For example, run a benchmark:

      ```
      python3 -m sglang.bench_serving --backend sglang --dataset-name random --num-prompts 100 --random-input 128 --random-output 128
      ```
  * Check the Prometheus UI (`http://localhost:9090`) under `Status` -> `Targets` to see if the SGLang endpoint is being scraped successfully.
  * Verify the `model_name` and `instance` labels in your Prometheus metrics match the variables used in the Grafana dashboard. You might need to adjust the Grafana dashboard variables or the labels in your Prometheus configuration.

#### Configuration Files

The monitoring setup is defined by the following files within the `examples/monitoring` directory:

* `docker-compose.yaml`: Defines the Prometheus and Grafana services.
* `prometheus.yaml`: Prometheus configuration, including scrape targets.
* `grafana/datasources/datasource.yaml`: Configures the Prometheus data source for Grafana.
* `grafana/dashboards/config/dashboard.yaml`: Tells Grafana to load dashboards from the specified path.
* `grafana/dashboards/json/sglang-dashboard.json`: The actual Grafana dashboard definition in JSON format.

You can customize the setup by modifying these files. For instance, you might need to update the `static_configs` target in `prometheus.yaml` if your SGLang server runs on a different host or port.

