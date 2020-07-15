# Overview

This is a Helm chart that can install Keycloak with a Postgres database. The Postgres database is managed by an Postgres database operator and you can use either the [Crunchydata](https://github.com/CrunchyData/postgres-operator) or the [Zalando](https://github.com/zalando/postgres-operator) Postgres operator.

# Prerequisites

* An OpenShift 4.4 cluster. OperatorHub is installed on the cluster (it should be by default) and Keycloak operator is available from the OperatorHub (it should be there by default)
* [Helm](https://helm.sh/)

# Usage

1. Login to the cluster as a cluster-admin user using `oc`.
2. Then run the following commands:
   ```
   oc new-project keycloak
   # You can either use the crunchdata postgres operator by running the following cmd:
   helm install keycloak . --set databases.crunchydata=true --set databases.zalando=false
   # Or using the Zalando postgres operator:
   helm install keycloak . --set databases.crunchydata=false --set databases.zalando=true
   ```
   You can run both of them on the same cluster, just use different projects in this case.

# Known Issues

* It will take a few minutes for Keycloak pods to get ready. If the pods are not ready after a while and they are consistently getting killed by OpenShift, you should be able to fix that by doing the following:
  * Scale down the keycloak-operator deployment
  * Update the Keycloak StatefulSet configuration to increase the `failureThreshold` for `readinessProbe`. Increase it a value like 10. Save the update.
  * Delete the existing keycloak pods. The StatefulSet will create them again.
