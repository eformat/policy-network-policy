# policy-network-policy

- [Common base](input/base)
- [All Non Control Plane Namespace Overlay](input/overlay/all-namespaces)

Generate NetworkPolicy to meet OCP-CIS 5.3.2

- https://github.com/ComplianceAsCode/content/blob/master/applications/openshift/networking/configure_network_policies_namespaces/rule.yml

CLI:

```bash
kustomize build --enable-helm --enable-alpha-plugins .
```

Or when using GitOps:

```bash
oc apply -f- <<'EOF'
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: network-policy
  namespace: open-cluster-management-global-set
  labels:
    rht-gitops.com/open-cluster-management-global-set: policies
spec:
  goTemplate: true
  generators:
  - clusterDecisionResource:
      configMapRef: acm-placement
      labelSelector:
        matchLabels:
          cluster.open-cluster-management.io/placement: placement-all-openshift
      requeueAfterSeconds: 180
  syncPolicy:
    preserveResourcesOnDeletion: false
  template:
    metadata:
      name: network-policy-{{.name}}
      annotations:
        argocd.argoproj.io/compare-options: IgnoreExtraneous
    spec:
      project: default
      destination:
        server: "{{.server}}"
        namespace: open-cluster-management-global-set
      source:
        repoURL: https://github.com/eformat/policy-network-policy
        path: .
        targetRevision: main
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
EOF
```
