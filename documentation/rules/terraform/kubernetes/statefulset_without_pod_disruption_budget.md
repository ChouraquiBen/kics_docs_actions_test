---
title: "StatefulSet Without PodDisruptionBudget"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/statefulset_without_pod_disruption_budget"
  id: "7249e3b0-9231-4af3-bc5f-5daf4988ecbf"
  display_name: "StatefulSet Without PodDisruptionBudget"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Availability"
---
## Metadata

**Id:** `7249e3b0-9231-4af3-bc5f-5daf4988ecbf`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Availability

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/stateful_set#selector)

### Description

 StatefulSets should be assigned with a PodDisruptionBudget to ensure high availability


## Compliant Code Examples
```terraform
resource "kubernetes_stateful_set" "prometheus2" {
  metadata {
    annotations = {
      SomeAnnotation = "foobar"
    }

    labels = {
      k8s-app                           = "prometheus"
      "kubernetes.io/cluster-service"   = "true"
      "addonmanager.kubernetes.io/mode" = "Reconcile"
      version                           = "v2.2.1"
    }

    name = "prometheus"
  }

  spec {
    pod_management_policy  = "Parallel"
    replicas               = 2
    revision_history_limit = 5

    selector {
      match_labels = {
        k8s-app = "prometheus"
      }
    }

    service_name = "prometheus"

    template {
      metadata {
        labels = {
          k8s-app = "prometheus"
        }

        annotations = {}
      }

      spec {
        service_account_name = "prometheus"

        init_container {
          name              = "init-chown-data"
          image             = "busybox:latest"
          image_pull_policy = "IfNotPresent"
          command           = ["chown", "-R", "65534:65534", "/data"]

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }
        }

        container {
          name              = "prometheus-server-configmap-reload"
          image             = "jimmidyson/configmap-reload:v0.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--volume-dir=/etc/config",
            "--webhook-url=http://localhost:9090/-/reload",
          ]

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
            read_only  = true
          }

          resources {
            limits = {
              cpu    = "10m"
              memory = "10Mi"
            }

            requests = {
              cpu    = "10m"
              memory = "10Mi"
            }
          }
        }

        container {
          name              = "prometheus-server"
          image             = "prom/prometheus:v2.2.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--config.file=/etc/config/prometheus.yml",
            "--storage.tsdb.path=/data",
            "--web.console.libraries=/etc/prometheus/console_libraries",
            "--web.console.templates=/etc/prometheus/consoles",
            "--web.enable-lifecycle",
          ]

          port {
            container_port = 9090
          }

          resources {
            limits = {
              cpu    = "200m"
              memory = "1000Mi"
            }

            requests = {
              cpu    = "200m"
              memory = "1000Mi"
            }
          }

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
          }

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }

          readiness_probe {
            http_get {
              path = "/-/ready"
              port = 9090
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }

          liveness_probe {
            http_get {
              path   = "/-/healthy"
              port   = 9090
              scheme = "HTTPS"
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }
        }

        termination_grace_period_seconds = 300

        volume {
          name = "config-volume"

          config_map {
            name = "prometheus-config"
          }
        }
      }
    }

    update_strategy {
      type = "RollingUpdate"

      rolling_update {
        partition = 1
      }
    }

    volume_claim_template {
      metadata {
        name = "prometheus-data"
      }

      spec {
        access_modes       = ["ReadWriteOnce"]
        storage_class_name = "standard"

        resources {
          requests = {
            storage = "16Gi"
          }
        }
      }
    }
  }
}


resource "kubernetes_pod_disruption_budget" "demo2" {
  metadata {
    name = "demo"
  }
  spec {
    max_unavailable = "20%"
    selector {
      match_labels = {
        k8s-app = "prometheus"
      }
    }
  }
}


resource "kubernetes_stateful_set" "prometheus3" {
  metadata {
    annotations = {
      SomeAnnotation = "foobar"
    }

    labels = {
      k8s-app                           = "prometheus"
      "kubernetes.io/cluster-service"   = "true"
      "addonmanager.kubernetes.io/mode" = "Reconcile"
      version                           = "v2.2.1"
    }

    name = "prometheus"
  }

  spec {
    pod_management_policy  = "Parallel"
    replicas               = 2
    revision_history_limit = 5

    selector {
      match_labels = {
        k8s-app = "kubernetes_pod_disruption_budget.demo2.spec.selector.0.match_labels.k8s-app2"
      }
    }

    service_name = "prometheus"

    template {
      metadata {
        labels = {
          k8s-app = "prometheus"
        }

        annotations = {}
      }

      spec {
        service_account_name = "prometheus"

        init_container {
          name              = "init-chown-data"
          image             = "busybox:latest"
          image_pull_policy = "IfNotPresent"
          command           = ["chown", "-R", "65534:65534", "/data"]

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }
        }

        container {
          name              = "prometheus-server-configmap-reload"
          image             = "jimmidyson/configmap-reload:v0.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--volume-dir=/etc/config",
            "--webhook-url=http://localhost:9090/-/reload",
          ]

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
            read_only  = true
          }

          resources {
            limits = {
              cpu    = "10m"
              memory = "10Mi"
            }

            requests = {
              cpu    = "10m"
              memory = "10Mi"
            }
          }
        }

        container {
          name              = "prometheus-server"
          image             = "prom/prometheus:v2.2.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--config.file=/etc/config/prometheus.yml",
            "--storage.tsdb.path=/data",
            "--web.console.libraries=/etc/prometheus/console_libraries",
            "--web.console.templates=/etc/prometheus/consoles",
            "--web.enable-lifecycle",
          ]

          port {
            container_port = 9090
          }

          resources {
            limits = {
              cpu    = "200m"
              memory = "1000Mi"
            }

            requests = {
              cpu    = "200m"
              memory = "1000Mi"
            }
          }

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
          }

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }

          readiness_probe {
            http_get {
              path = "/-/ready"
              port = 9090
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }

          liveness_probe {
            http_get {
              path   = "/-/healthy"
              port   = 9090
              scheme = "HTTPS"
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }
        }

        termination_grace_period_seconds = 300

        volume {
          name = "config-volume"

          config_map {
            name = "prometheus-config"
          }
        }
      }
    }

    update_strategy {
      type = "RollingUpdate"

      rolling_update {
        partition = 1
      }
    }

    volume_claim_template {
      metadata {
        name = "prometheus-data"
      }

      spec {
        access_modes       = ["ReadWriteOnce"]
        storage_class_name = "standard"

        resources {
          requests = {
            storage = "16Gi"
          }
        }
      }
    }
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_stateful_set" "prometheus" {
  metadata {
    annotations = {
      SomeAnnotation = "foobar"
    }

    labels = {
      k8s-app                           = "prometheus"
      "kubernetes.io/cluster-service"   = "true"
      "addonmanager.kubernetes.io/mode" = "Reconcile"
      version                           = "v2.2.1"
    }

    name = "prometheus"
  }

  spec {
    pod_management_policy  = "Parallel"
    replicas               = 2
    revision_history_limit = 5

    selector {
      match_labels = {
        k8s-app2 = "prometheus2"
      }
    }

    service_name = "prometheus"

    template {
      metadata {
        labels = {
          k8s-app = "prometheus"
        }

        annotations = {}
      }

      spec {
        service_account_name = "prometheus"

        init_container {
          name              = "init-chown-data"
          image             = "busybox:latest"
          image_pull_policy = "IfNotPresent"
          command           = ["chown", "-R", "65534:65534", "/data"]

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }
        }

        container {
          name              = "prometheus-server-configmap-reload"
          image             = "jimmidyson/configmap-reload:v0.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--volume-dir=/etc/config",
            "--webhook-url=http://localhost:9090/-/reload",
          ]

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
            read_only  = true
          }

          resources {
            limits = {
              cpu    = "10m"
              memory = "10Mi"
            }

            requests = {
              cpu    = "10m"
              memory = "10Mi"
            }
          }
        }

        container {
          name              = "prometheus-server"
          image             = "prom/prometheus:v2.2.1"
          image_pull_policy = "IfNotPresent"

          args = [
            "--config.file=/etc/config/prometheus.yml",
            "--storage.tsdb.path=/data",
            "--web.console.libraries=/etc/prometheus/console_libraries",
            "--web.console.templates=/etc/prometheus/consoles",
            "--web.enable-lifecycle",
          ]

          port {
            container_port = 9090
          }

          resources {
            limits = {
              cpu    = "200m"
              memory = "1000Mi"
            }

            requests = {
              cpu    = "200m"
              memory = "1000Mi"
            }
          }

          volume_mount {
            name       = "config-volume"
            mount_path = "/etc/config"
          }

          volume_mount {
            name       = "prometheus-data"
            mount_path = "/data"
            sub_path   = ""
          }

          readiness_probe {
            http_get {
              path = "/-/ready"
              port = 9090
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }

          liveness_probe {
            http_get {
              path   = "/-/healthy"
              port   = 9090
              scheme = "HTTPS"
            }

            initial_delay_seconds = 30
            timeout_seconds       = 30
          }
        }

        termination_grace_period_seconds = 300

        volume {
          name = "config-volume"

          config_map {
            name = "prometheus-config"
          }
        }
      }
    }

    update_strategy {
      type = "RollingUpdate"

      rolling_update {
        partition = 1
      }
    }

    volume_claim_template {
      metadata {
        name = "prometheus-data"
      }

      spec {
        access_modes       = ["ReadWriteOnce"]
        storage_class_name = "standard"

        resources {
          requests = {
            storage = "16Gi"
          }
        }
      }
    }
  }
}


resource "kubernetes_pod_disruption_budget" "demo" {
  metadata {
    name = "demo"
  }
  spec {
    max_unavailable = "20%"
    selector {
      match_labels = {
        test = "MyExampleApp"
      }
    }
  }
}

```