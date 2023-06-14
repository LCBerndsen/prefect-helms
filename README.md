# Prefect Server Helm Chart

## Basics

Execute this to make sure you get all the dependencies defined in `Chart.yaml`

```shell
helm dependency update
```

Create a config.yaml that overrides values in `values.yaml` from the prefect-server chart

### Example basic config

This is an example for minikube deployment

```yaml
prefect-server:
  jobs:
    createTenant:
      enabled: true
      tenant:
        name: gerimedica
        slug: gerimedica
  agent:
    enabled: true
    replicas: 1
    env:
      - name: PREFECT__CLOUD__AGENT__LABELS
        value: '["gpu-server"]'
  apollo:
    service:
      type: ClusterIP
    ingress:
      enabled: true
      ingressClassName: nginx
      hosts:
        - prefect-apollo.minikube.io
  ui:
    apolloApiUrl: "http://prefect-apollo.minikube.io/graphql"
    service:
      type: ClusterIP
    ingress:
      enabled: true
      ingressClassName: nginx
      hosts:
        - prefect-ui.minikube.io
```

You can find more details about configuration [here](ADV_README.md)

### Deploy 

Make sure you are pointing to the right cluster

Modify the following values if it is needed

```shell
# The kubernetes namespace to install into, can be anything or excluded to install in the default namespace
NAMESPACE=prefect-server
# The Helm "release" name, can be anything but we recommend matching the chart name
NAME=prefect-server
# The path to your config that overrides values in `values.yaml`
CONFIG_PATH=config.yaml

helm upgrade --install \
    $NAME \
    -n $NAMESPACE \
    --values $CONFIG_PATH \
    prefecthq/prefect-server
```

## Advance info

you can get more details how to deploy prefect from the original [README](charts%2FREADME.md)