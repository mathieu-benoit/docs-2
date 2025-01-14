---
title: "Score schema reference"
linkTitle: "Schema reference"
weight: 10
description: >
    Validate your Score file with the Score schema.
---

The Score schema is a JSON schema that defines the structure of a Score file. It's used to validate the Score file before an implementation CLI (such as `score-compose` or `score-helm`) is executed. The Score implementation CLI validates the Score file against the schema before generating the platform-specific configuration, by default.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://score.dev/schemas/score",
  "title": "Score schema",
  "description": "Score workload specification",
  "type": "object",
  "required": [
    "apiVersion",
    "metadata",
    "containers"
  ],
  "additionalProperties": false,
  "properties": {
    "apiVersion": {
      "description": "The declared Score Specification version.",
      "type": "string"
    },
    "metadata": {
      "description": "The metadata description of the Workload.",
      "type": "object",
      "required": [
        "name"
      ],
      "additionalProperties": true,
      "properties": {
        "name": {
          "description": "A string that can describe the Workload.",
          "type": "string"
        }
      }
    },
    "service": {
      "description": "The service that the workload provides.",
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "ports": {
          "description": "List of network ports published by the service.",
          "type": "object",
          "additionalProperties": true,
          "minProperties": 1,
          "patternProperties": {
            "^*": {
              "description": "The network port description.",
              "type": "object",
              "required": [
                "targetPort"
              ],
              "additionalProperties": false,
              "properties": {
                "port": {
                  "description": "The public service port.",
                  "type": "number"
                },
                "targetPort": {
                  "description": "The internal service port.",
                  "type": "number"
                }
              }
            }
          }
        }
      }
    },
    "containers": {
      "description": "The declared Score Specification version.",
      "type": "object",
      "additionalProperties": true,
      "minProperties": 1,
      "patternProperties": {
        "^*": {
          "description": "The container name.",
          "type": "object",
          "required": [
            "image"
          ],
          "additionalProperties": false,
          "properties": {
            "image": {
              "description": "The image name and tag.",
              "type": "string"
            },
            "command": {
              "description": "If specified, overrides container entry point.",
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "string"
              }
            },
            "args": {
              "description": "If specified, overrides container entry point arguments.",
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "string"
              }
            },
            "variables": {
              "description": "The environment variables for the container.",
              "type": "object",
              "minProperties": 1,
              "additionalProperties": { "type": "string" }
            },
            "files": {
              "description": "The extra files to mount.",
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "object",
                "required": [
                  "target",
                  "content"
                ],
                "properties": {
                  "target": {
                    "description": "The file path and name.",
                    "type": "string"
                  },
                  "mode": {
                    "description": "The file access mode.",
                    "type": "string"
                  },
                  "content": {
                    "description": "The inline content for the file.",
                    "type": "array",
                    "minItems": 1,
                    "items": {
                      "type": "string"
                    }
                  }
                }
              }
            },
            "volumes": {
              "description": "The volumes to mount.",
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "object",
                "required": [
                  "source",
                  "target"
                ],
                "properties": {
                  "source": {
                    "description": "The external volume reference.",
                    "type": "string"
                  },
                  "path": {
                    "description": "An optional sub path in the volume.",
                    "type": "string"
                  },
                  "target": {
                    "description": "The target mount on the container.",
                    "type": "string"
                  },
                  "read_only": {
                    "description": "Indicates if the volume should be mounted in a read-only mode.",
                    "type": "boolean"
                  }
                }
              }
            },
            "resources": {
              "description": "The compute resources for the container.",
              "type": "object",
              "minProperties": 1,
              "additionalProperties": false,
              "properties": {
                "limits": {
                  "description": "The maximum allowed resources for the container.",
                  "$ref": "#/properties/containers/definitions/resourcesLimits"
                },
                "requests": {
                  "description": "The minimal resources required for the container.",
                  "$ref": "#/properties/containers/definitions/resourcesLimits"
                }
              }
            },
            "livenessProbe": {
              "description": "The liveness probe for the container.",
              "type": "object",
              "minProperties": 1,
              "additionalProperties": false,
              "properties": {
                "httpGet": {
                  "description": "",
                  "$ref": "#/properties/containers/definitions/httpProbe"
                }
              }
            },
            "readinessProbe": {
              "description": "The readiness probe for the container.",
              "type": "object",
              "minProperties": 1,
              "additionalProperties": false,
              "properties": {
                "httpGet": {
                  "description": "",
                  "$ref": "#/properties/containers/definitions/httpProbe"
                }
              }
            }
          }
        }
      },
      "definitions": {
        "resourcesLimits": {
          "description": "The compute resources limits.",
          "type": "object",
          "minProperties": 1,
          "additionalProperties": false,
          "properties": {
            "memory": {
              "description": "The memory limit.",
              "type": "string"
            },
            "cpu": {
              "description": "The CPU limit.",
              "type": "string"
            }
          }
        },
        "httpProbe": {
          "description": "An HTTP probe details.",
          "type": "object",
          "additionalProperties": false,
          "required": [
            "path"
          ],
          "properties": {
            "path": {
              "description": "The path of the HTTP probe endpoint.",
              "type": "string"
            },
            "port": {
              "description": "The path of the HTTP probe endpoint.",
              "type": "number"
            },
            "httpHeaders": {
              "description": "Additional HTTP headers to send with the request",
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "object",
                "additionalProperties": false,
                "properties": {
                  "name": {
                    "description": "The HTTP header name.",
                    "type": "string"
                  },
                  "value": {
                    "description": "The HTTP header value.",
                    "type": "string"
                  }
                }
              }
            }
          }
        }
      }
    },
    "resources": {
      "description": "The dependencies needed by the Workload.",
      "type": "object",
      "minProperties": 1,
      "additionalProperties": true,
      "patternProperties": {
        "^*": {
          "description": "The resource name.",
          "type": "object",
          "additionalProperties": false,
          "required": [
            "type"
          ],
          "properties": {
            "type": {
              "description": "The resource in the target environment.",
              "type": "string"
            },
            "metadata": {
              "description": "The metadata for the resource.",
              "type": "object",
              "minProperties": 1,
              "additionalProperties": true,
              "properties": {
                "annotations": {
                  "description": "Annotattions that apply to the property.",
                  "type": "object",
                  "minProperties": 1,
                  "additionalProperties": { "type": "string" }
                }
              }
            },
            "properties": {
              "description": "The properties that can be referenced in other places in the Score Specification file.",
              "type": "object",
              "minProperties": 1,
              "patternProperties": {
                "^*": {
                  "description": "The property name.",
                  "type": ["object", "null"],
                  "minProperties": 1,
                  "additionalProperties": false,
                  "properties": {
                    "type": {
                      "description": "The property value type.",
                      "type": "string"
                    },
                    "default": {
                      "description": "A value that applies for the property by default.",
                      "type": ["integer", "string", "boolean"]
                    },
                    "required": {
                      "description": "Indictes if the property value is requred.",
                      "type": "boolean"
                    },
                    "secret": {
                      "description": "Indicates if the property value contains sensitive information.",
                      "type": "boolean"
                    }
                  }
                }
              }
            },
            "params": {
              "description": "The parameters used to validate or provision the resource in the environment.",
              "type": "object"
            }
          }
        }
      }
    }
  }
}
```