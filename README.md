# argocd-rhacm
Use ArgoCD openshift-gitops to deploy and "gitops" RHACM manifests instea of the native ACM Application/Subscription.

It is focused on Azure Dev Ops as a git repository and using ArgoCD as the only tool to deliver Applications.  That is, without any use of ACM Channels, Subscriptions or Applications.  Without use ACM Applications even in delivering ACM's own manfiests for operation.  It will use ArgoCD and Application Sets only for all Application instantiation as well.