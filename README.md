# Application Monitoring Operator

A Kubernetes Operator based on the Operator SDK that installs the Integr8ly Application Monitoring Stack.

# Current status

This is a PoC / alpha version. Most functionality is there but it is highly likely there are bugs and improvements needed.

# Supported Custom Resources

The following Grafana resources are supported:

* ApplicationMonitoring

## ApplicationMonitoring

Triggers the installation of the monitoring stack when created. This is achieved by deploying two other operators that install Prometheus and Grafana respectively.

# Installation

You will need cluster admin permissions to create CRDs, ClusterRoles & ClusterRoleBindings.
ClusterRoles are needed to allow the operators to watch multiple namespaces.

```
oc new-project application-monitoring
```

Grafana CRDs

```
oc apply -f https://raw.githubusercontent.com/integr8ly/grafana-operator/master/deploy/crds/Grafana.yaml
oc apply -f https://raw.githubusercontent.com/integr8ly/grafana-operator/master/deploy/crds/GrafanaDashboard.yaml
```

Cluster Roles & RoleBindings

```
oc apply -f ./deploy/roles
```

Application Monitoring Operator

```
oc apply -f ./deploy/operator_roles/
oc apply -f ./deploy/crds/ApplicationMonitoring.yaml
oc apply -f ./deploy/operator.yaml
oc apply -f ./deploy/examples/ApplicationMonitoring.yaml
```

# Running locally (for development)

You can run the Operator locally against a remote namespace. The name of the namespace should be `application-monitoring`. To run the operator execute:

```sh
$ make setup/dep
$ make code/run
```