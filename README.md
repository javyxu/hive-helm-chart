# Hive Helm Chart

## What is it
A [helm chart](https://helm.sh/) to deploy [helm-metastore](https://cwiki.apache.org/confluence/display/Hive/AdminManual+Metastore+Administration) on kuberentes.

This chart is designed to deploy the image [javy/hive](https://hub.docker.com/repository/docker/xujavy/hive), containing **hive versin 2.3.9**, and **hadoop version 2.10.2**. The current image uses Postgres as external metadata (external metadata is needed to allow scaling in prodution kubernetes environments) and supports [AWS S3](https://aws.amazon.com/pt/s3/) (and compatible object storages like [MinIO](https://min.io/), if not supported yet (if can create and entry on helm chart, please, create a pull request).

## Configurtions

Helm chart properties are the following:


| Option                                        | Description                                                                                                                                                                        | Defualt Value                    |
|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------|
| postgres.host                                 | Postgres metadata host (hostname or ip)                                                                                                                                            |                                  |
| postgres.port                                 | Postgres metadata port                                                                                                                                                             | 5432                             |
| postgres.database                             | Postgres metadata database name                                                                                                                                                    | hive_metastore                   |
| postgres.username.rawValue                    | Postgres metadata database username                                                                                                                                                | postgres                         |
| postgres.password.rawValue                    | Postgres metadata database username                                                                                                                                                | postgres                         |
| autoCreateSchema                              | Value for hive property datanucleus.autoCreateSchema  | false                            |
| warehouseDir                                  | Value for hive property hive.metastore.warehouse.dir  | file:///tmp  |  |
| blobStorages.s3aConfiguration.accessKeyId     | AWS Access key ID to authenticate S3 storage. |                                  |
| blobStorages.s3aConfiguration.secretAccessKey | AWS Secret Access Key to authenticate S3 storage.  |                                  |
| blobStorages.s3aConfiguration.endpoint        | AWS Endpoint, can be used to connect on other object storages like MinIO and LakeFS. If empty, connect to regular AWS cloud endpoints                                              | <AWS S3 Cloud Standard endpoint> |
| blobStorages.s3aConfiguration.pathStyleAccess | Value for key fs.s3a.path.style.access      |                                  |
| blobStorages.s3aConfiguration.s3aWarehousePath | Value for key fs.defaultFS      |                                  |
| blobStorages.s3aConfiguration.sslEnabled | Value for key fs.s3a.connection.ssl.enabled      |                                  |
| hiveConfigParameters                          | Used to map any other hive metastore configutation. Is an array of objects like  ```{key: <hive metastore configuration/property>, value: <Value for specified configuration> }``` |                                  |
| image.repository                              | Te image repository with hive metastore to be deployed.                                                                                                                            | andreclaudino/hive-metastore     |
| image.tag                                     | The image tag/version to be deployed.                                                                                                                                              | 1.0.0                            |
| image.pullPolicy                              | The kubernetes policy to pull image.                                                                                                                                               | IfNotPresent                     |
| imagePullSecrets                              | Array of secret names to authenticate if image is on a private registry                                                                                                            | []                               |
| replicaCount                                  | Number of instances to be initially deployeed                                                                                                                                      | 1                                |
| autoscaling.enabled                           | If deployment should autoscale when starving resources                                                                                                                             | false                            |
| autoscaling.minReplicas                       | Minimum number of replicas to keep when downscaling                                                                                                                                | 1                                |
| autoscaling.maxReplicas                       | Maximum number of replicas to keep when upscaling                                                                                                                                  | 100                              |
| autoscaling.targetCPUUtilizationPercentage    | Percentage of CPU to trigger autoscale                                                                                                                                             |                                  |
| autoscaling.targetMemoryUtilizationPercentage | Percentage of used Memory to trigger autoscale                                                                                                                                     |                                  |