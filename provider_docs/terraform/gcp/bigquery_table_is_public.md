---
title: "BigQuery Table Is Public"
meta:
  name: "terraform/bigquery_table_is_public"
  id: "a9b8c7d6-e5f4-3210-abcd-1234567890ab"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/bigquery_table_is_public`
**Id:** `a9b8c7d6-e5f4-3210-abcd-1234567890ab`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Insecure Configurations
## Description
When BigQuery tables are configured with public access through IAM members or bindings using principals like 'allUsers' or 'allAuthenticatedUsers', they expose potentially sensitive data to anyone on the internet or any authenticated Google account. This significantly increases the risk of data breaches, unauthorized access, and compliance violations related to data privacy regulations.

To secure BigQuery tables, always restrict access to specific authenticated users, service accounts, or groups instead of using public principals. For example, use `user:someone@example.com` instead of `allUsers` or `allAuthenticatedUsers` as shown in this comparison:

```hcl
// Insecure configuration
resource "google_bigquery_table_iam_member" "insecure_example" {
  dataset_id = "my_dataset"
  table_id   = "my_table"
  member     = "allUsers"
  role       = "roles/bigquery.dataViewer"
}

// Secure configuration
resource "google_bigquery_table_iam_member" "secure_example" {
  dataset_id = "my_dataset"
  table_id   = "my_table"
  member     = "user:someone@example.com"
  role       = "roles/bigquery.dataViewer"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/bigquery_table_iam_member)

## Non-Compliant Code Examples
```gcp
# IAM Member violation
resource "google_bigquery_table_iam_member" "bad_example_member" {
  table  = "example_table"
  member = "allUsers" # ❌ Public principal
  role   = "roles/bigquery.dataViewer"
}

# IAM Binding violation
resource "google_bigquery_table_iam_binding" "bad_example_binding" {
  table   = "example_table"
  members = ["allAuthenticatedUsers", "user:someone@example.com"] # ❌ Contains public principal
  role    = "roles/bigquery.dataViewer"
}

```

## Compliant Code Examples
```gcp
# IAM Binding compliant
resource "google_bigquery_table_iam_binding" "good_example_binding" {
  table   = "example_table"
  members = ["user:someone@example.com", "group:admins@example.com"] # ✅ No public principals
  role    = "roles/bigquery.dataViewer"
}

```

```gcp
# IAM Member compliant
resource "google_bigquery_table_iam_member" "good_example_member" {
  table  = "example_table"
  member = "user:someone@example.com" # ✅ Non-public principal
  role   = "roles/bigquery.dataViewer"
}

```