# alfresco-sync-service

![Version: 4.0.0](https://img.shields.io/badge/Version-4.0.0-informational?style=flat-square) ![AppVersion: 4.0.0-M7](https://img.shields.io/badge/AppVersion-4.0.0--M7-informational?style=flat-square)

Alfresco Sync Service

## Source Code

* <https://github.com/Alfresco/acs-deployment>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://alfresco.github.io/alfresco-helm-charts/ | messageBroker(activemq) | 3.0.1 |
| https://alfresco.github.io/alfresco-helm-charts/ | alfresco-common | 2.0.0 |
| oci://registry-1.docker.io/bitnamicharts | common | 2.x.x |
| oci://registry-1.docker.io/bitnamicharts | database(postgresql) | 12.x.x |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| database | object | `{"auth":{"database":"alfrescosync","enablePostgresUser":false,"password":"admin","username":"alfresco"},"enabled":false,"external":{"driver":"org.postgresql.Driver","existingSecretName":null,"password":"admin","url":null,"user":"alfresco"},"nameOverride":"postgresql-syncservice","primary":{"extendedConfiguration":"shared_buffers = 256MB\nmax_connections = 80\neffective_cache_size = 1024GB\nlog_min_messages = LOG\n"},"resources":{"limits":{"cpu":"2","memory":"2Gi"}}}` | Defines properties required by sync service for connecting to the database If you set database.external to true you will have to setup the JDBC driver, user, password and JdbcUrl as `driver`, `user`, `password` & `url` subelements of `database`. Also make sure that the container has the db driver |
| database.enabled | bool | `false` | If set to `true` a dedicated postgres instance will be deployed in the cluster for sync-service to use it. When set to `false` the chart expects you provide DB configuration details. |
| database.external.driver | string | `"org.postgresql.Driver"` | The JDBC Driver to connect to the DB. If different from the default make sure your container image ships it. |
| database.external.existingSecretName | string | `nil` | An existing kubernetes secret with DB info (prefered over using values) |
| database.external.password | string | `"admin"` | JDBC password to use to connect to the DB |
| database.external.url | string | `nil` | JDBC url to connect to the external DB. Required if `.database.enabled` is set to `true` |
| database.external.user | string | `"alfresco"` | JDBC username to use to connect to the DB |
| environment.EXTRA_JAVA_OPTS | string | `""` |  |
| environment.JAVA_OPTS | string | `"-Dsync.metrics.reporter.graphite.enabled=false -XX:MinRAMPercentage=50 -XX:MaxRAMPercentage=80"` |  |
| global | object | `{"alfrescoRegistryPullSecrets":"quay-registry-secret","strategy":{"rollingUpdate":{"maxSurge":1,"maxUnavailable":0}}}` | Global definition of Docker registry pull secret which can be overridden from parent ACS Helm chart(s) |
| image.internalPort | int | `9090` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"quay.io/alfresco/service-sync"` |  |
| image.tag | string | `"4.0.0-M7"` |  |
| ingress.extraAnnotations | string | `nil` | useful when running Sync service without SSL termination done by a load balancer, e.g. when ran on Minikube for testing purposes nginx.ingress.kubernetes.io/ssl-redirect: "false" |
| ingress.path | string | `"/syncservice"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.initialDelaySeconds | int | `30` |  |
| livenessProbe.periodSeconds | int | `30` |  |
| livenessProbe.timeoutSeconds | int | `10` |  |
| messageBroker | object | `{"adminUser":{"password":null,"user":null},"enabled":false,"external":{"existingSecretName":null,"password":"admin","url":null,"user":"alfresco"},"nameOverride":"activemq","services":{"broker":{"ports":{"external":{"openwire":61616}}}}}` | messageBroker object allow to pass ActiveMQ connection details. url: provides URI formatted string, see: https://activemq.apache.org/failover-transport-reference user: username to authenticate as. password: credential to use to authenticate to the broker. |
| messageBroker.adminUser.password | string | `nil` | Password to use to set as the connection user for ActiveMQ |
| messageBroker.adminUser.user | string | `nil` | User to use to set as the connection user for ActiveMQ |
| messageBroker.external.existingSecretName | string | `nil` | An existing kubernetes secret with MQ info (prefered over using values) |
| nodeSelector | object | `{}` |  |
| podSecurityContext.fsGroup | int | `1000` |  |
| podSecurityContext.runAsGroup | int | `1000` |  |
| podSecurityContext.runAsNonRoot | bool | `true` |  |
| podSecurityContext.runAsUser | int | `33020` |  |
| readinessProbe.failureThreshold | int | `12` |  |
| readinessProbe.initialDelaySeconds | int | `20` |  |
| readinessProbe.periodSeconds | int | `10` |  |
| readinessProbe.timeoutSeconds | int | `10` |  |
| replicaCount | int | `1` |  |
| repository.host | string | `"alfresco-cs-repository"` |  |
| repository.port | int | `80` |  |
| resources.limits.cpu | string | `"2"` |  |
| resources.limits.memory | string | `"2000Mi"` |  |
| resources.requests.cpu | string | `"0.5"` |  |
| resources.requests.memory | string | `"800Mi"` |  |
| service.externalPort | int | `80` |  |
| service.name | string | `"syncservice"` |  |
| service.type | string | `"NodePort"` |  |

Please refer to the [documentation](https://github.com/Alfresco/acs-deployment/blob/master/docs/helm/README.md) for information on the Helm charts and deployment instructions.