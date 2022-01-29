# Monitoring and Telemetry for Production System

### **Monitoring & Telemetry for Production Systems**

**Monitoring** provides feedback from production. Monitoring delivers information about an application’s performance and usage patterns.

One goal of monitoring is to achieve high availability by minimizing time to detect and time to mitigate (TTD, TTM). In other words, as soon as performance and other issues arise, rich diagnostic data about the issues are fed back to development teams via automated monitoring. That’s TTD. DevOps teams act on the information to mitigate the issues as quickly as possible so that users are no longer affected. That’s TTM. Resolution times are measured, and teams work to improve over time. After mitigation, teams work on how to remediate problems at root cause so that they do not recur. That time is measured as TTR.

A second goal of monitoring is to enable “validated learning” by tracking usage. The core concept of validated learning is that every deployment is an opportunity to track experimental results that support or diminish the hypotheses that led to the deployment. Tracking usage and differences between versions allows teams to measure the impact of change and drive business decisions. If a hypothesis is diminished, the team can “fail fast” or “pivot”. If the hypothesis is supported, then the team can double down or “persevere”. These data-informed decisions lead to new hypotheses and prioritization of the backlog.

**Telemetry** is the mechanism for collecting data from monitoring. Telemetry can use agents that are installed in the deployment environments, an SDK that relies on markers inserted into source code, server logging, or a combination of these. Typically, telemetry will distinguish between the data pipeline optimized for real-time alerting and dashboards and higher-volume data needed for troubleshooting or usage analytics.

Monitoring is often used to “test in production”. A well-monitored deployment streams the data about its health and performance so that the team can spot production incidents immediately. Combined with a Continuous Deployment Release Pipeline, monitoring will detect new anomalies and allow for prompt mitigation. This allows discovery of the “unknown unknowns” in application behavior that cannot be foreseen in pre-production environments.

_Effective monitoring is essential to allow DevOps teams to deliver at speed, get feedback from production, and increase customers satisfaction, acquisition and retention._

Here are some monitoring terminologies that you will find useful:

**Server monitoring** tools help users identify and solve any application hosting and performance issues by tracking and monitoring server performance. Monitoring provides visibility and insight into the performance of various servers in your IT infrastructure.

**Server**: In computing, a server is a piece of computer hardware or software that provides functionality for other programs or devices, called "clients". This architecture is called the client–server model.

**Cloud:** Cloud computing is the on-demand availability of computer system resources, especially data storage and computing power, without direct active management by the user. The term is generally used to describe data centers available to many users over the Internet.

**Microservices** - also known as the microservice architecture - is an architectural style that structures an application as a collection of services that are

* Highly maintainable and testable
* Loosely coupled
* Independently deployable
* Organized around business capabilities
* Owned by a small team

The microservice architecture enables the rapid, frequent and reliable delivery of large, complex applications. It also enables an organization to evolve its technology stack.

**Spring Boot Microservices** are microservice applications built using the Java Spring boot framework which provides a set of features to quickly build microservices. This framework allows microservices to working together and can quickly setup services, with minimal configurations.

**Application Performance Monitoring (APM)**: An area of IT that focuses on monitoring the performance of software application programs in order to provide end-users with a quality experience.

**Prometheus:** Originally developed by SoundCloud is an open source and community-driven project that graduated from the Cloud Native Computing Foundation. It can aggregate data from almost everything Microservices, Multiple languages, Linux servers, Windows servers which can be used for generating visual representations with Grafana.

**Grafana** is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources like Prometheus.

**Docker** is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels.

**cAdvisor** (short for container Advisor) analyzes and exposes resource usage and performance data from running containers (Docker).

**ELK stack** is an acronym used to describe a stack that comprises of three popular open-source projects: Elasticsearch, Logstash, and Kibana. Often referred to as Elasticsearch, the ELK stack gives you the ability to aggregate logs from all your systems and applications, analyze these logs, and create visualizations for application and infrastructure monitoring, faster troubleshooting, security analytics, and more.&#x20;

E = **Elasticsearch** Elasticsearch is an open-source, RESTful, distributed search and analytics engine built on Apache Lucene. Support for various languages, high performance, and schema-free JSON documents makes Elasticsearch an ideal choice for various log analytics and search use cases.&#x20;

L = **Logstash** Logstash is an open-source data ingestion tool that allows you to collect data from a variety of sources, transform it, and send it to your desired destination. With pre-built filters and support for over 200 plugins, Logstash allows users to easily ingest data regardless of the data source or type.&#x20;

K = **Kibana** Kibana is an open-source data visualization and exploration tool for reviewing logs and events. Kibana offers easy-to-use, interactive charts, pre-built aggregations and filters, and geospatial support and making it the preferred choice for visualizing data stored in Elasticsearch.

Reference  : &#x20;

* [https://github.com/BINPIPE/monitoring-and-telemetry](https://github.com/BINPIPE/monitoring-and-telemetry)

