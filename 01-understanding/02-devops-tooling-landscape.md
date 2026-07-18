# Chapter 02: The DevOps Tooling Landscape

This manual explores the common tools used in each phase of the DevOps continuous delivery pipeline.

---

## 1. The Tooling Map

DevOps practitioners select tools to automate transitions between the development and operations lifecycles.

```
  +------------+-------------------------------------------------+
  | Phase      | Common Industrial Tools                         |
  +------------+-------------------------------------------------+
  | **PLAN**   | Jira, Confluence, Trello, Slack                 |
  | **CODE**   | Git, GitHub, GitLab, VS Code                    |
  | **BUILD**  | Maven, Gradle, npm, pip, MSBuild                |
  | **TEST**   | JUnit, Selenium, SonarQube (Static Analysis)    |
  | **DEPLOY** | Jenkins, GitHub Actions, Ansible, Terraform     |
  | **RUN**    | Docker, Kubernetes, AWS, Azure, GCP             |
  | **MONITOR**| Prometheus, Grafana, ELK Stack, Splunk          |
  +------------+-------------------------------------------------+
```

---

## 2. Tool Categories Explained

### 2.1 Code & Version Control
* **Git**: The local CLI tool to track file changes.
* **GitHub / GitLab / Bitbucket**: Hosting platforms that store code repositories and enable collaborative reviews.

### 2.2 Continuous Integration & Orchestration (CI)
* **Jenkins**: An open-source automation server that compiles code, runs tests, and builds packaging containers.
* **GitHub Actions**: Integrated YAML-based CI pipelines directly within GitHub.

### 2.3 Containerization & Orchestration
* **Docker**: Packages applications into isolated, portable containers.
* **Kubernetes (K8s)**: Manages cluster scaling, load balancing, and container lifetimes.

### 2.4 Infrastructure as Code (IaC) & Configuration Management
* **Terraform**: Declares cloud server infrastructures (VMs, networks, databases) via YAML-like text files.
* **Ansible**: Configures software applications and OS packages across remote servers automatically over SSH.

### 2.5 Monitoring & Log Management
* **Prometheus**: Collects hardware and software performance metrics.
* **Grafana**: Visualizes those metrics using beautiful dashboards.
* **ELK Stack (Elasticsearch, Logstash, Kibana)**: Gathers, indexes, and searches raw logs from multiple containers in one place.
