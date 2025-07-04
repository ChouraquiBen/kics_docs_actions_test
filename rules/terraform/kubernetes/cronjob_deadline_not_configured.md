---
title: "CronJob Deadline Not Configured"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/cronjob_deadline_not_configured"
  id: "58876b44-a690-4e9f-9214-7735fa0dd15d"
  display_name: "CronJob Deadline Not Configured"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Resource Management"
---
## Metadata

**Id:** `58876b44-a690-4e9f-9214-7735fa0dd15d`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Resource Management

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/cron_job#starting_deadline_seconds)

### Description

 Cronjobs must have a configured deadline, which means the attribute 'starting_deadline_seconds' must be defined


## Compliant Code Examples
```terraform
resource "kubernetes_cron_job" "demo2" {
  metadata {
    name = "demo"
  }
  spec {
    concurrency_policy            = "Replace"
    failed_jobs_history_limit     = 5
    schedule                      = "1 0 * * *"
    starting_deadline_seconds     = 10
    successful_jobs_history_limit = 10
    job_template {
      metadata {}
      spec {
        backoff_limit              = 2
        ttl_seconds_after_finished = 10
        template {
          metadata {}
          spec {
            container {
              name    = "hello"
              image   = "busybox"
              command = ["/bin/sh", "-c", "date; echo Hello from the Kubernetes cluster"]
            }
          }
        }
      }
    }
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_cron_job" "demo" {
  metadata {
    name = "demo"
  }
  spec {
    concurrency_policy            = "Replace"
    failed_jobs_history_limit     = 5
    schedule                      = "1 0 * * *"
    successful_jobs_history_limit = 10
    job_template {
      metadata {}
      spec {
        backoff_limit              = 2
        ttl_seconds_after_finished = 10
        template {
          metadata {}
          spec {
            container {
              name    = "hello"
              image   = "busybox"
              command = ["/bin/sh", "-c", "date; echo Hello from the Kubernetes cluster"]
            }
          }
        }
      }
    }
  }
}

```