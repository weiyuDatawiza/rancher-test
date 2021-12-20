# Access Broker Helm Chart

* Installs the Identity Aware proxy [Access Broker](https://www.datawiza.com/access-broker)

## Get Repo Info

```console
helm repo add datawiza https://datawiza-inc.github.io/helm-charts/
helm repo update
```

## Installing the Chart

Please follow the [doc](https://docs.datawiza.com/step-by-step/step2.html) to create an application on the Datawiza Cloud Management Console (DCMC) to generate a pair of `PROVISIONING_KEY`, `PROVISIONING_SECRET`, and the command line to log in to our docker repo.

Use the command line to log in and create a Kubernetes Secret based on the Docker credentials. You can see [here](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/) for more details.

Then, create a yaml file named `example.yaml` based on these values:

```yaml
PROVISIONING_KEY: replace-with-your-provisioning-key
PROVISIONING_SECRET: replace-with-your-provisioning-key
containerPort: replace-with-your-app-listen-port
imagePullSecrets: replace-with-you-secret
```

To install the chart with the release name `my-release`:

```console
helm install my-release -f example.yaml datawiza/access-broker
```

## Uninstalling the Chart

To uninstall/delete the my-release deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

### Example ingress with path

```yaml
PROVISIONING_KEY: replace-with-your-provisioning-key
PROVISIONING_SECRET: replace-with-your-provisioning-key
containerPort: 9772
imagePullSecrets: replace-with-you-secret
service:
  type: ClusterIP
  port: 9772
ingress:
  annotations:
    kubernetes.io/ingress.class: nginx
  className: ''
  enabled: true
  hosts:
    - host: accessbroke.my.com
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: replace-with-you-svc
              port:
                number: 9772
```
