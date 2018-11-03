
# Run a local kubernetes cluster on MacOS

The official [dashboard wiki] is the source of truth.  There's a nice issue / thread ([kubernetes/dashboard: 2474](https://github.com/kubernetes/dashboard/issues/2474)for some background if you're really curious about why your normal kubeconfig file will likely not work.  TLDR: you need a token, not a cert.

[dashboard wiki]: https://github.com/kubernetes/dashboard/wiki

##  Set your context

```bash
# kctx docker-for-desktop
kubectl config use-context docker-for-desktop
```

## deploy the kubernetes dashboard

Deploy directly from github HEAD...

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```

or

```bash
git clone git@github.com:kubernetes/dashboard.git && cd dashboard
kubectl create -f src/deploy/recommended/kubernetes-dashboard.yaml
```

Example:

```shell
(⎈ |docker-for-desktop:default)halcyondude@chezmoi:~/k8s$ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
secret/kubernetes-dashboard-certs created
serviceaccount/kubernetes-dashboard created
role.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
deployment.apps/kubernetes-dashboard created
service/kubernetes-dashboard created
```

## Create a user with rights to the dashboard

_taken from <https://github.com/kubernetes/dashboard/wiki/Creating-sample-user>_

### Create ServiceAccount and ClusterRoleBinding

*Warning! The following enables cluster-admin access to the service account being created.  This could enable escalation of privilege and
other potential issues.  It is NOT a secure way configure a cluster.

Please see [Admin privileges] section of 

[Admin privileges]: https://github.com/kubernetes/dashboard/wiki/Access-control#admin-privileges

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard-admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: dashboard-admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: dashboard-admin-user
  namespace: kube-system
```

Apply [kubernetes-dashboard-admin-user.yaml](kubernetes-dashboard-admin-user.yaml)
to create an account with cluster-admin

```bash
kubectl apply -f kubernetes-dashboard-admin-user.yaml
```

## Obtain a token

The dashboard cannot use a certificate, it needs an token.  See the [access control] portion of the wiki for details.

[access control]: https://github.com/kubernetes/dashboard/wiki/Access-control#admin-privileges

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
```

yields:

```text
Name:         dashboard-admin-user-token-8gddxNamespace:    kube-systemLabels:       <none>Annotations:  kubernetes.io/service-account.name: dashboard-admin-user
              kubernetes.io/service-account.uid: 1e0e8639-df1c-11e8-92e8-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYXNoYm9hcmQtYWRtaW4tdXNlci10b2tlbi04Z2RkeCIsImt1YmVybmV0ZXMuaW8vc2Vyd
mljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJkYXNoYm9hcmQtYWRtaW4tdXNlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjFlMGU4NjM5LWRmMWMtMTFlOC05MmU4LTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTpkYXNoYm9hcmQtYWRtaW4tdXNlciJ9.CE1Byq0NxmWTF4
O80_6POZhO7w_zshVU8PuU_d2xKHqe9opMRB7
```

Copy the token from here, or (*BETTER*) emit the results as json (using `get` vs `describe`) like this:

```bash
kubectl -n kube-system get secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') -o json
```

```json
{
    "apiVersion": "v1",
    "data": {
        "ca.crt": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNE1UQXdOREUyTlRrME5sb1hEVEk0TVRBd01URTJOVGswTmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVB
BRENDQVFvQ2dnRUJBTWpjCnY4b05tTnNwUisvN2lONXRRZ2tXdWZLdGF3L1ZRVlFLS29GYXg4VU9NKzc2VDVZOWtkeGpyOGtHVU04ajFJQ0sKdkpjSXAvbXNOYTcxenVKK1JmMFlxQVZMVFlTYXZuUUUxUkxPRnEwcVFRSVhxVjFDZzM2bVJYcEFwT0ZKV0dMbwowbnhrbnJ1ZmluOW51SDdwcHNDdThYSlR5cHJCYXV1NFZZK2xTWldreEpLK3FFSGJTTmFvUm5uUlEwWEZqd2plCjU3T2hQSlBRY3gyZWtKOFZYZ21WMDF2RVpoOVgybnRLdVJib0kxcWh4aGJxc1YzaEIvWlEwdk5ZTUJNalBQS08KeEJ2aHhoMWRJTE85YUVBWms1VkNFMmYvZ0djOFh2NnJ4dVQrREJ6WXhrQkh0V282cFBPb2I3TUVIbzlWRThtegpQK2U2eU9TVEt3T1RMN1g3WEEwQ0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFCdzFpSXV2dVhWOVl2T0poZVpTbUYrUlZxVW8KK29rYjNYQlBZL2NBMEF5MHVubkhLWGRWcUZ2d09uSzNpOHhqcTlkTE9kWDhEK21HVUpKQUhYbi92ZllnSVhycwpUbDJIbnNBdGpyK2lwdVlFWGtmSE5rMHZXVHRJWHZTVUxPL0ZRbnZ4YjgvQStwb2p0alorREJPVTVLbldlNXhYCmMrN2VIN0poa0RlekE0eFJaemZuQ0MyTXllSy9laSt2cGo4clNQSXVtTnJPMGxKMTFYNUlvNXc5OHJmUHlEajMKZ05QMHZkWmtnbGtwdUFKR2tSMnJKaFV4VVhrYkQ1ODU3RlNNREZOUlIwc3E3cVprdnlOaXhsTVY5WGtLSjgrNgo3SERVUk01anRNd1liKzRBYitROHVadm1GdVJ5UEN3Vnk0NVBnQ2ZrVFliNEFvT2Flak9lU0k1b3JGOD0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=",        "namespace": "a3ViZS1zeXN0ZW0=",        "token": "ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNklpSjkuZXlKcGMzTWlPaUpyZFdKbGNtNWxkR1Z6TDNObGNuWnBZMlZoWTJOdmRXNTBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5dVlXMWxjM0JoWTJVaU9pSnJkV0psTFhONWMzUmxiU0lzSW10MVltVnlibVYwWlhNdWFXOHZjMlZ5ZG1salpXRmpZMjkxYm5RdmMyVmpjbVYwTG01aGJXVWlPaUprWVhOb1ltOWhjbVF0WVdSdGFXNHRkWE5sY2kxMGIydGxiaTA0WjJSa2VDSXNJbXQxWW1WeWJtVjBaWE11YVc4dmMyVnlkbWxqWldGalkyOTFiblF2YzJWeWRtbGpaUzFoWTJOdmRXNTBMbTVoYldVaU9pSmtZWE5vWW05aGNtUXRZV1J0YVc0dGRYTmxjaUlzSW10MVltVnlibVYwWlhNdWFXOHZjMlZ5ZG1salpXRmpZMjkxYm5RdmMyVnlkbWxqWlMxaFkyTnZkVzUwTG5WcFpDSTZJakZsTUdVNE5qTTVMV1JtTVdNdE1URmxPQzA1TW1VNExUQXlOVEF3TURBd01EQXdNU0lzSW5OMVlpSTZJbk41YzNSbGJUcHpaWEoyYVdObFlXTmpiM1Z1ZERwcmRXSmxMWE41YzNSbGJUcGtZWE5vWW05aGNtUXRZV1J0YVc0dGRYTmxjaUo5LkNFMUJ5cTBOeG1XVEY0TzgwXzZQT1poTzd3X3pzaFZVOFB1VV9kMnhLSHFlOW9wTVJCN3EyRkQtUGcwaGE5eXR0amY2M3BYTkQyeEFhS2VFMnVlNWNSZUluSVV4RU04NUdpSjFOcm9JTlBQMWFtRWJraC1YbVVTeVVrSFVLbzYwNWt6cjZ2N2VGeGIwNUNEa0pHOUFqajhDdnpEemRURHFSalp6MEtUWE15YjhPLWY5c1ktcUp3V2hZQTdWaUxCVXBfNjJPeUFBZE9qWDdrWU1CQU1IWmM0VlNNQ1dreUZfTG84SzFHeF9YVXE5NzNxT1Vvbk9rcTZlbjN2MHpXdXBodTFoWmRjakxLVm1mR2toLXpmWE1KSkdtUF8zUEk2M0Fod0JLWXFGRFJfNjN1d2l0Qmd3SVl3OFd4STkwb2phWk1uVmhuT3o5eEhxZ3plOG05WDZZdw=="    },    "kind": "Secret",
    "metadata": {        "annotations": {            "kubernetes.io/service-account.name": "dashboard-admin-user",            "kubernetes.io/service-account.uid": "1e0e8639-df1c-11e8-92e8-025000000001"        },
        "creationTimestamp": "2018-11-03T03:54:09Z",
        "name": "dashboard-admin-user-token-8gddx",
        "namespace": "kube-system",
        "resourceVersion": "11308",
        "selfLink": "/api/v1/namespaces/kube-system/secrets/dashboard-admin-user-token-8gddx",
        "uid": "1e11942f-df1c-11e8-92e8-025000000001"
    },
    "type": "kubernetes.io/service-account-token"
}
```

or (*BEST*), use jsonpath to pull out the value we want and store it in a local file.

```bash
kubectl -n kube-system get secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') -o jsonpath='{.data.token}' > dashboard-admin-user.token
```

## run the proxy

```bash
kubectl proxy
```

## Connect to the dashboard using token

```bash
open http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
```

## 

---

### janky way

kubectl get pod --namespace=kube-system | grep dashboard

Use port forwarding... https://localhost:8443

kubectl port-forward kubernetes-dashboard-7798c48646–2cdd7 8443:8443 --namespace=kube-system

## or...better
kubectl create -f dashboard-service.yaml

## get port
kubectl get service --namespace=kube-system | grep kubernetes-dashboard-nodeport
