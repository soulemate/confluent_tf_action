---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "confluent_subject_compatibility_level Data Source - terraform-provider-confluent"
subcategory: ""
description: |-
  
---

# confluent_subject_compatibility_level Data Source

[![Open Preview](https://img.shields.io/badge/Lifecycle%20Stage-Open%20Preview-%2300afba)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy)

-> **Note:** `confluent_subject_compatibility_level` data source is available in **Open Preview** for early adopters. Open Preview features are introduced to gather customer feedback. This feature should be used only for evaluation and non-production testing purposes or to provide feedback to Confluent, particularly as it becomes more widely available in follow-on editions.  
**Open Preview** features are intended for evaluation use in development and testing environments only, and not for production use. The warranty, SLA, and Support Services provisions of your agreement with Confluent do not apply to Open Preview features. Open Preview features are considered to be a Proof of Concept as defined in the Confluent Cloud Terms of Service. Confluent may discontinue providing preview releases of the Open Preview features at any time in Confluent’s sole discretion.

`confluent_subject_compatibility_level` describes a Subject Config data source.

## Example Usage

### Option #1: Manage multiple Schema Registry clusters in the same Terraform workspace

```terraform
provider "confluent" {
  cloud_api_key    = var.confluent_cloud_api_key    # optionally use CONFLUENT_CLOUD_API_KEY env var
  cloud_api_secret = var.confluent_cloud_api_secret # optionally use CONFLUENT_CLOUD_API_SECRET env var
}

data "confluent_subject_compatibility_level" "example" {
  schema_registry_cluster {
    id = confluent_schema_registry_region.essentials.id
  }
  rest_endpoint = confluent_schema_registry_region.essentials.rest_endpoint
  subject_name = "proto-purchase-value"
  credentials {
    key    = "<Schema Registry API Key for confluent_schema_registry_region.essentials>"
    secret = "<Schema Registry API Secret for confluent_schema_registry_region.essentials>"
  }
}

output "compatibility_level" {
  value = data.confluent_subject_compatibility_level.example.compatibility_level
}
```

### Option #2: Manage a single Schema Registry cluster in the same Terraform workspace

```terraform
provider "confluent" {
  schema_registry_id            = var.schema_registry_id            # optionally use SCHEMA_REGISTRY_ID env var
  schema_registry_rest_endpoint = var.schema_registry_rest_endpoint # optionally use SCHEMA_REGISTRY_REST_ENDPOINT env var
  schema_registry_api_key       = var.schema_registry_api_key       # optionally use SCHEMA_REGISTRY_API_KEY env var
  schema_registry_api_secret    = var.schema_registry_api_secret    # optionally use SCHEMA_REGISTRY_API_SECRET env var
}

data "confluent_subject_compatibility_level" "example" {
  subject_name = "proto-purchase-value"
}

output "compatibility_level" {
  value = data.confluent_subject_compatibility_level.example.compatibility_level
}
```

<!-- schema generated by tfplugindocs -->
## Argument Reference

The following arguments are supported:

- `schema_registry_cluster` - (Optional Configuration Block) supports the following:
  - `id` - (Required String) The ID of the Schema Registry cluster, for example, `lsrc-abc123`.
- `rest_endpoint` - (Optional String) The REST endpoint of the Schema Registry cluster, for example, `https://psrc-00000.us-central1.gcp.confluent.cloud:443`).
- `credentials` (Optional Configuration Block) supports the following:
  - `key` - (Required String) The Schema Registry API Key.
  - `secret` - (Required String, Sensitive) The Schema Registry API Secret.
- `subject_name` - (Required String) The name of the subject (in other words, the namespace), representing the subject under which the schema will be registered, for example, `test-subject`.

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `id` - (Required String) The ID of the Subject Config, in the format `<Schema Registry cluster ID>/<Subject name>`, for example, `lsrc-abc123/test-subject`.
- `compatibility_level` - (Required String) The Compatibility Level of the specified subject. Accepted values are: `BACKWARD`, `BACKWARD_TRANSITIVE`, `FORWARD`, `FORWARD_TRANSITIVE`, `FULL`, `FULL_TRANSITIVE`, and `NONE`. See the [Compatibility Types](https://docs.confluent.io/platform/current/schema-registry/avro.html#compatibility-types) for more details.
