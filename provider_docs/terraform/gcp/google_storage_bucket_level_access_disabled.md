---
title: "Google Storage Bucket Level Access Disabled"
meta:
  name: "terraform/google_storage_bucket_level_access_disabled"
  id: "bb0db090-5509-4853-a827-75ced0b3caa0"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/google_storage_bucket_level_access_disabled`
**Id:** `bb0db090-5509-4853-a827-75ced0b3caa0`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Insecure Configurations
## Description
Google Storage Bucket Level Access controls access to objects at the bucket level rather than allowing fine-grained permissions at the object level. When disabled, Access Control Lists (ACLs) can be used to grant permissions to individual objects, increasing the risk of accidental exposure or misconfiguration that could lead to unauthorized access to sensitive data.

Enabling uniform bucket-level access simplifies permissions management and helps ensure consistent access control across all objects in a bucket. To secure your configuration, set 'uniform_bucket_level_access = true' in your google_storage_bucket resource as shown:

```
resource "google_storage_bucket" "secure_bucket" {
  name          = "image-store.com"
  location      = "EU"
  
  uniform_bucket_level_access = true
  
  // other configuration...
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/storage_bucket)

## Non-Compliant Code Examples
```gcp
resource "google_storage_bucket" "positive1" {
  name          = "image-store.com"
  location      = "EU"
  force_destroy = true

  uniform_bucket_level_access = false

  website {
    main_page_suffix = "index.html"
    not_found_page   = "404.html"
  }
  cors {
    origin          = ["http://image-store.com"]
    method          = ["GET", "HEAD", "PUT", "POST", "DELETE"]
    response_header = ["*"]
    max_age_seconds = 3600
  }
}

resource "google_storage_bucket" "positive2" {
  name          = "image-store.com"
  location      = "EU"
  force_destroy = true

  website {
    main_page_suffix = "index.html"
    not_found_page   = "404.html"
  }
  cors {
    origin          = ["http://image-store.com"]
    method          = ["GET", "HEAD", "PUT", "POST", "DELETE"]
    response_header = ["*"]
    max_age_seconds = 3600
  }
}
```

## Compliant Code Examples
```gcp
resource "google_storage_bucket" "negative1" {
  name          = "image-store.com"
  location      = "EU"
  force_destroy = true

  uniform_bucket_level_access = true

  website {
    main_page_suffix = "index.html"
    not_found_page   = "404.html"
  }
  cors {
    origin          = ["http://image-store.com"]
    method          = ["GET", "HEAD", "PUT", "POST", "DELETE"]
    response_header = ["*"]
    max_age_seconds = 3600
  }
}
```