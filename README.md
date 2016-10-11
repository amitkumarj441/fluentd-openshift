# Fluentd logger

This repo is a variant of [Origin Aggregated Logging](https://github.com/openshift/origin-aggregated-logging).
It contains the image definitions of the components of the light-weight logging
stack as well as tools for building and deploying them.

The assumption is that we're using external ElasticSearch and visualization tools.

## Components

### Fluentd node collector

Fluentd is responsible for gathering log entries from nodes, enriching
them with metadata, and feeding them into the normalizer.

### Fluentd normalizer

Fluentd normalizer is responsible for aggregation of logs from node collectors,
normalizing them and forwarding to ElasticSearch.

### Fluentd kubernetes collector

Fluentd collector that is responsible for collection of logs from Kubernetes/ OpenShift.

### Ansible roles

Ansible playbooks/roles to deploy all of the above.

# Modifying configs

To modify config - change the config files in the directory and re-run ansible
playbook.

# Container image

Container image is built from [fluentd](fluentd/) folder.
Built container image is usually available as `t0ffel/logging-fluentd`.
Container image _does not_ contain any fluentd configuration files.
All configuration files must be passed in as ConfigMap resources.
