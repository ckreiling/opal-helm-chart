<p  align="center">
 <img src="https://i.ibb.co/BGVBmMK/opal.png" height=100 alt="opal" border="0" />
</p>
<h2 align="center">
OPAL Helm Chart for Kubernetes
</h2>

OPAL is an administration layer for Open Policy Agent (OPA), detecting changes to both policy and policy data in realtime and pushing live updates to your agents.

OPAL brings open-policy up to the speed needed by live applications. As your application state changes (whether it's via your APIs, DBs, git, S3 or 3rd-party SaaS services), OPAL will make sure your services are always in sync with the authorization data and policy they need (and only those they need).

[Check out OPAL main repo here.](https://github.com/authorizon/opal)

### Installation

OPAL Helm chart could be installed only with [Helm 3](https://helm.sh/docs/).
The chart is published to public Helm repository, [hosted on GitHub itself](https://authorizon.github.io/opal-helm-chart/). It's recommended to install OPAL into a dedicated namespace.

Add Helm repository

```
helm repo add opal https://authorizon.github.io/opal-helm-chart
helm repo update
```

Install the latest version

```
helm install --create-namespace -n opal-ns myopal opal/opal
```

Search for all available versions

```
helm search repo opal --versions
```

### Deploy OPAL to your Kubernetes cluster

Install specific version (with default configuration):

```
helm install --create-namespace -n opal-ns --version x.x.x myopal opal/opal
```

Install specific version (with custom configuration provided as YAML):

```
helm install -f myvalues.yaml --create-namespace -n opal-ns --version x.x.x myopal opal/opal
```

`myvalues.yaml` must conform to the [json schema](https://raw.githubusercontent.com/authorizon/opal-helm-chart/master/values.schema.json).

### Verify installation

OPAL Client should populate embedded OPA instance with polices and data from configured Git repository.
To validate it - one could create port-forwarding to OPAL client Pod. Port 8181 is the embedded OPA agent.

```
kubectl port-forward -n opal-ns service/opal-client 8181:8181
```

Then, open http://localhost:8181/v1/data/ in your browser to check OPA data document state.

