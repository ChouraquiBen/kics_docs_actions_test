---
title: "Ensure SQL Database Instance Has Skip Show Database Flag"
meta:
  name: "terraform/sql_database_instance_does_not_have_skip_show_database"
  id: "a8b7c6d5-e4f3-2109-8a7b-6c5d4e3f2109"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/sql_database_instance_does_not_have_skip_show_database`
**Id:** `a8b7c6d5-e4f3-2109-8a7b-6c5d4e3f2109`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
The absence of the `skip_show_database` flag or its incorrect setting within a `google_sql_database_instance` resource can allow users to view a list of all databases on a MySQL server instance, potentially exposing sensitive schema information to unauthorized individuals. This misconfiguration increases the risk of information disclosure and can aid attackers in reconnaissance activities by providing insight into database names and structures. To mitigate this risk, ensure the configuration includes `database_flags { name = "skip_show_database" value = "on" }` as shown below:

```
resource "google_sql_database_instance" "good_example" {
  name             = "good-instance"
  database_version = "MYSQL_8"
  region           = "us-central1"

  settings {
    tier = "db-custom-2-13312"
    database_flags {
      name  = "skip_show_database"
      value = "on"
    }
    database_flags {
      name  = "cross db ownership chaining"
      value = "on"
    }
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_database_instance)

## Non-Compliant Code Examples
```gcp
resource "google_sql_database_instance" "bad_example" {
  name             = "bad-instance"
  database_version = "MYSQL_8"
  region           = "us-central1"

  settings {
    tier = "db-custom-2-13312"
    database_flags {
      name  = "cross db ownership chaining"
      value = "on"
    }
  }
}

resource "google_sql_database_instance" "bad_example_2" {
  name             = "bad-instance"
  database_version = "MYSQL_8"
  region           = "us-central1"

  settings {
    tier = "db-custom-2-13312"
    database_flags {
      name  = "skip_show_database"
      value = "off"
    }
  }
}

resource "google_sql_database_instance" "bad_example_3" {
  name             = "bad-instance"
  database_version = "MYSQL_8"
  region           = "us-central1"

  settings {
    tier = "db-custom-2-13312"
  }
}

```

## Compliant Code Examples
```gcp
resource "google_sql_database_instance" "good_example" {
  name             = "good-instance"
  database_version = "MYSQL_8"
  region           = "us-central1"

  settings {
    tier = "db-custom-2-13312"
    database_flags {
      name  = "skip_show_database"
      value = "on" # This flag is present as required
    }
    database_flags {
      name  = "cross db ownership chaining"
      value = "on"
    }
  }
}

```