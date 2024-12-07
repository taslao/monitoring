# Work In Progress

## **Centralized Monitoring and Logging Stack**

This repository sets up a complete monitoring and logging stack with the following components:

1. **Prometheus**: Metrics collection and monitoring.
2. **Grafana**: Visualization platform for metrics and logs.
3. **Loki**: Log aggregation and querying.
4. **Promtail**: Log shipper designed to integrate seamlessly with Loki, responsible for collecting logs from Docker containers and the host system.

### **What It Does**
- **Prometheus** scrapes and stores metrics from Loki and itself.
- **Loki** aggregates and indexes logs for querying, storing the data in an efficient, scalable format.
- **Promtail** collects logs from containers and the host, enriching them with metadata, and forwards them to Loki.
- **Grafana** visualizes both logs and metrics, allowing you to create customizable dashboards.

---

## **Deployment Instructions**

### **Pre-requisites**
- Docker and Docker Compose installed on your system.

### **Steps to Deploy**
1. Clone this repository:
   ```shell
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Create a secret for the Grafana admin password:
   ```shell
   mkdir -p secrets
   echo "yourpassword" > secrets/admin_password
   ```

3. Start the stack using Docker Compose:
   ```shell
   docker compose up -d
   ```

4. Verify that all services are running:
   ```shell
   docker compose ps
   ```

5. Access each service using the URLs provided below.

---

## **Accessing Services**

| **Service**   | **Description**                      | **Browser URL**                                |
|---------------|--------------------------------------|------------------------------------------------|
| **Prometheus** | Metrics collection and monitoring    | [http://localhost:9090](http://localhost:9090) |
| **Grafana**    | Dashboard visualization              | [http://localhost:3000](http://localhost:3000) |
| **Loki**       | Log aggregation (API only)           | [http://loki:3001](http://loki:3001)           |
| **Promtail**   | Log collection and shipping          | N/A                                            |

- **Grafana Login**:
   - Username: `admin`
   - Password: Value stored in `secrets/admin_password`.

---

## **Comparison of Log Shippers**

| **Feature / Requirement** | **Fluentd**           | **Filebeat**        | **Syslog-ng/Rsyslog** | **Promtail**        |
|---------------------------|-----------------------|---------------------|-----------------------|---------------------|
| **Best Integration with Loki** | ✅ Native integration via plugin | ⚠ Requires Loki plugin | ❌ No native integration | ✅ Designed for Loki |
| **Log Enrichment (Adding Metadata)** | ✅ Highly customizable with plugins | ⚠ Limited enrichment; JSON supported | ⚠ Metadata via manual config | ✅ Automatic metadata for containers |
| **Supports JSON Logs**    | ✅ Full support        | ✅ Full support      | ✅ Requires setup      | ✅ Full support      |
| **Lightweight**           | ⚠ Resource-intensive  | ✅ Lightweight       | ✅ Lightweight         | ✅ Lightweight       |
| **Complexity**            | ⚠ High; plugin-driven | ✅ Easy to configure | ⚠ Manual parsing      | ✅ Easy to configure |
| **Kubernetes/Docker Metadata** | ✅ Full support       | ⚠ Requires setup     | ❌ Minimal             | ✅ Automatic         |

**Recommendation**: Use **Promtail** with Loki for seamless integration and simplicity.

---

## **Customizing Configuration**

### **Promtail**
- Configuration is located in `oci/promtail/conf/promtail-config.yaml`.
- Updates can be made to add new log sources or enrich metadata.

### **Loki**
- Configuration is located in `oci/loki/conf/loki-config.yaml`.
- Adjust retention periods and storage backends as needed.

### **Prometheus**
- Configuration is located in `oci/prometheus/conf/prometheus.yml`.
- Add additional scrape targets for services you wish to monitor.

### **Grafana**
- Custom Grafana settings can be defined in `oci/grafana/conf/custom.ini`.
- Data sources and dashboards can be pre-configured using provisioning files.

---

## **Contributing**
1. Fork this repository.
2. Create a feature branch.
3. Commit your changes and push to your fork.
4. Open a pull request.

---

