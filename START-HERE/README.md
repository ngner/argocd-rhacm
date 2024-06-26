# This is the root application

These resources will deploy OpenShift GitOps operator and have it bootstrap everything in an App of Apps style.

First find and replace my repo URL with yours `grep -r argocd-rhacm ../`

```bash
echo "password={yourgit repo token string for Argo to use}" >> gitsecret.env"
oc apply -k ./START-HERE
```

It will create the namespace for the GitOps operator etc.

Once complete then we can deploy the root Application or App of Apps, this provides the resource to have GitOps connect to this Git Repo and start applying the resources to the hub cluster.

The resrouces applied to the Hub Cluster will in turn, through services like ACM and OCP GitOps etc, start deploying clusters, deplying operators and configiuation and applications to those clusters.

You will need to create a credential to access the Git Repository.  Use the token creation function in ADO or other GIt to create Token then set it as the value for `password=` in gisecret.env.
Gather your main vault key, this should enable access to a vault-of-vaults which will hold other vaults keys for the External Secrets Operators on the remote clusters set this as the value for `secret-id=` in central-vault-key-secret.env.

```bash
echo "password={yourgit repo token string for Argo to use}" >> gitsecret.env
echo "secret-id={secret-id={vault key secret for ESO}" >> central-vault-key-secret.env

```

## Deploy the root application

The below can be avoided by simply running `oc apply -k .` then waiting and trying again.

We need to wait for Git Ops operator to complete before we can deploy an argoproj/Application resource.  The resource type is not recognised until Install is complete.

```bash
oc apply -k ./START-HERE/app-of-apps/gitops-root-app.yaml
```

## TBC still - what further RBAC roles and perms required

[Configuring clusters with ArgoCD ref docs](https://docs.openshift.com/container-platform/4.11/cicd/gitops/configuring-an-openshift-cluster-by-deploying-an-application-with-cluster-configurations.html)

Assess the use of one cluster scoped service account for all argoCD instances instead have more specific ones for e.g. policy and workload app-of-apps

Limiting further what resources the root-apps can deploy through app-project.

Applying App-projects for all root-apps.
