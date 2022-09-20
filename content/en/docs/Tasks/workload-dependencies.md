---
title: "Set Workload dependencies"
linkTitle: "Workload dependencies"
weight: 4
---

The PAWS specification may define one or more dependencies required by the workload to run. This could include other workloads, third party services, configuration sets or external resources such as databases and volumes for example.

Workload dependancies are described declaratively. The PAWS specification does not define how to configure or provision any dependancy but may include additional metadata (name, type, labels) that allows to identify and link it to the platform that consumes the output file generated based on paws.yaml to deploy the workload at runtime.

For example, the PAWS specification might declare a `postgres` database as a required dependency for the workload:

- When run with `docker-compose`, a `postgres` service should be defined in an external `compose.yaml` file and started with `compose up` command before the workload is run.
- When run with `Humanitec`, a `postgres` resource should be configured for the target environment so that the `Humanitec` controller can match and provision it before the workload is deployed into the according Kubernetes cluster.