---
title: "KMS Crypto Key is Publicly Accessible"
meta:
  name: "terraform/kms_crypto_key_publicly_accessible"
  id: "16cc87d1-dd47-4f46-b3ce-4dfcac8fd2f5"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/kms_crypto_key_publicly_accessible`
**Id:** `16cc87d1-dd47-4f46-b3ce-4dfcac8fd2f5`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Encryption
## Description
Google Cloud KMS Crypto Keys provide cryptographic functionality for encrypting and decrypting sensitive data in Google Cloud. When KMS Crypto Key IAM policies include 'allUsers' or 'allAuthenticatedUsers', they become publicly accessible, creating a serious security vulnerability that could lead to unauthorized access to encryption capabilities, data breaches, or compromised encrypted information.

Insecure configuration example:
```
data "google_iam_policy" {
  binding {
    role = "roles/cloudkms.cryptoKeyEncrypter"
    members = ["allUsers"]
  }
}
```

Secure configuration with specific user access:
```
data "google_iam_policy" {
  binding {
    role = "roles/cloudkms.cryptoKeyEncrypter"
    members = [
      "user:jane@example.com",
    ]
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_kms_crypto_key_iam#google_kms_crypto_key_iam_policy)

## Non-Compliant Code Examples
```gcp
resource "google_kms_key_ring" "positive1" {
  name     = "keyring-example"
  location = "global"
}
resource "google_kms_crypto_key" "positive1" {
  name            = "crypto-key-example"
  key_ring        = google_kms_key_ring.positive1.id
  rotation_period = "100000s"
  lifecycle {
    prevent_destroy = true
  }
}

data "google_iam_policy" "positive1" {
  binding {
    role = "roles/cloudkms.cryptoKeyEncrypter"

    member = "allUsers"
  }
}

resource "google_kms_crypto_key_iam_policy" "positive1" {
  crypto_key_id = google_kms_crypto_key.positive1.id
  policy_data = data.google_iam_policy.positive1.policy_data
}

```

```gcp
resource "google_kms_key_ring" "positive2" {
  name     = "keyring-example"
  location = "global"
}
resource "google_kms_crypto_key" "positive2" {
  name            = "crypto-key-example"
  key_ring        = google_kms_key_ring.positive2.id
  rotation_period = "100000s"
  lifecycle {
    prevent_destroy = true
  }
}

data "google_iam_policy" "positive2" {
  binding {
    role = "roles/cloudkms.cryptoKeyEncrypter"

    member = "allAuthenticatedUsers"
  }
}

resource "google_kms_crypto_key_iam_policy" "positive2" {
  crypto_key_id = google_kms_crypto_key.keyyy.id
  policy_data = data.google_iam_policy.positive2.policy_data
}

```

## Compliant Code Examples
```gcp
resource "google_kms_key_ring" "negative" {
  name     = "negative-example"
  location = "global"
}
resource "google_kms_crypto_key" "negative" {
  name            = "crypto-key-example"
  key_ring        = google_kms_key_ring.negative.id
  rotation_period = "100000s"
  lifecycle {
    prevent_destroy = true
  }
}

data "google_iam_policy" "negative" {
  binding {
    role = "roles/cloudkms.cryptoKeyEncrypter"

    members = [
      "user:jane@example.com",
    ]
  }
}

resource "google_kms_crypto_key_iam_policy" "negative" {
  crypto_key_id = google_kms_crypto_key.negative.id
  policy_data = data.google_iam_policy.negative.policy_data
}

```