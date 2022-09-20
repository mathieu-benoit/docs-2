---
title: "Docker Compose"
linkTitle: "Docker"
weight: 4
description: >
  Learn to translate a Score Specification file into a Docker Compose configuration with the target Platform CLI tool.
---

Use `score-docker` to run the target Platform CLI tool.

The target Platform CLI tool takes in the Score Specification file and translates it into a Docker Compose configuration.

This section will get you started with transforming a Score Specification file into a Docker Compose file and share important information on the target Platform CLI tool.

To get started with `score-docker` create a directory and enter the following information into a file called `paws.yaml`.

```yaml Hello world
name: hello-world
containers:
  hello:
    image: busybox
```

From your terminal, run the following command.

```bash
paws-compose run
```

By default, `--file` defaults to `./paws.yaml` and `--output` defaults to `./compose.yaml`.

The following is the expected output from the previous command.

```bash
Reading './paws.yaml'...
Parsing PAWS spec...
Building docker-compose configuration...
Creating './compose.yaml'...
Writing docker-compose configuration...
```

The following is the output of the run command.

```yaml
services:
  hello-world: {}
```

**Results**: You've successfully transformed a Score Specification file into a Docker Compose configuration.