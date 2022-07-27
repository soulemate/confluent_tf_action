### Notes

1. This example assumes that Terraform is run from a host in the private network, where it will have connectivity to the Kafka REST API. If it is not, you must make these changes:

  * Update the `confluent_api_key` resources by setting their `disable_wait_for_ready` flag to `true`. Otherwise, Terraform will attempt to validate API key creation by listing topics, which will fail without access to the Kafka REST API. Check the [Kafka REST API docs](https://docs.confluent.io/cloud/current/api.html#tag/Topic-(v3)) to learn more about it. Otherwise, you might see errors like:

    ```
    Error: error waiting for Kafka API Key "[REDACTED]" to sync: error listing Kafka Topics using Kafka API Key "[REDACTED]": Get "[https://[REDACTED]/kafka/v3/clusters/[REDACTED]/topics](https://[REDACTED]/kafka/v3/clusters/[REDACTED]/topics)": GET [https://[REDACTED]/kafka/v3/clusters/[REDACTED]/topics](https://[REDACTED]/kafka/v3/clusters/[REDACTED]/topics) giving up after 5 attempt(s): Get "[https://[REDACTED]/kafka/v3/clusters/[REDACTED]/topics](https://[REDACTED]/kafka/v3/clusters/[REDACTED/topics)": dial tcp [REDACTED]:443: i/o timeout
    ```

  * Remove the three `confluent_kafka_acl` resources. These resources are provisioned using the Kafka REST API, which is only accessible from the private network.

2. See [VPC Peering on Google Cloud](https://docs.confluent.io/cloud/current/networking/peering/gcp-peering.html) for more details.
