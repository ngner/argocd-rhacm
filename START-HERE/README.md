# This is the root application

These resources will deploy OpenShift GitOps operator and have it bootstrap everything in an App of Apps style.

simply run

```bash
oc apply -k ./START-HERE
```

It will create the namespace for the GitOps operator etc.

Once complete then we can deploy the root Application or App of Apps, this provides the resource to have GitOps connect to this Git Repo and start applying the resources to the hub cluster.

The resrouces applied to the Hub Cluster will in turn, through services like ACM and OCP GitOps etc, start deploying clusters, deplying operators and configiuation and applications to those clusters.

## TODO - curently use branch `main` until the merge policies are agreed

You will need to create a credential to access the Repository.  Use the token creation function in ADO to create Token for use then either use the GUI to deploy or the following template for a secret DON'T CHECK IT IN TO GIT !!!

## Deploy the root application

We need to wait for Git Ops operator to complete before we can deploy an argoproj/Application resource.  The resource type is not recognised until Install is complete.

```bash
oc apply -k ./START-HERE/app-of-apps/gitops-root-app.yaml
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: git-repo-creds
  namespace: openshift-gitops
  labels:
    argocd.argoproj.io/secret-type: repo-creds
stringData:
  type: git
  url: https://github.com/ngner/argocd-rhacm.git
  #password: TOKEN-FROM-Gitlab
  #username: gitlab+deploy-token-{some-number-from-gitlab}
```

## TBC still - what further RBAC roles and perms required

[Configuring clusters with ArgoCD ref docs](https://docs.openshift.com/container-platform/4.11/cicd/gitops/configuring-an-openshift-cluster-by-deploying-an-application-with-cluster-configurations.html)

Added to this are the apiGroups open-cluster-management.io

NOTE: Namespaces which will be managed by the ArgoCD must have label `argocd.argoproj.io/managed-by=openshift-gitops`
