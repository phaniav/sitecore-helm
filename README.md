# Sitecore Helm charts

Add this repo:

```shell
helm repo add sitecoreops https://raw.githubusercontent.com/sitecoreops/sitecore-helm/master/repo
```

Show available charts

```shell
helm repo update
helm search repo sitecore
```

## Changelog

### April 2020

- [Added] Sitecore 9.3 XM chart `sitecore930-xm`.

## Examples

### Minimal deployment (no ingress)

```powershell
# create a new namespace for your deployment
$namespace = "awesome-sitecore-project"

kubectl create namespace $namespace

# deploy your Sitecore license file
kubectl create secret generic "license.xml" --from-file "C:\license\license.xml" --namespace $namespace

# install Sitecore XM (reference: https://helm.sh/docs/intro/using_helm/)
helm upgrade release1 sitecoreops/sitecore930-xm --install --namespace $namespace `
    --set solr.image.tag="<IMAGE TAG OR DIGEST>" `
    --set sql.image.tag="<IMAGE TAG OR DIGEST>" `
    --set cd.image.tag="<IMAGE TAG OR DIGEST>" `
    --set cm.image.tag="<IMAGE TAG OR DIGEST>" `
    --set cm.adminPassword="<INSERT SECRET HERE>" `
    --set global.sqlSaPassword="<INSERT SECRET HERE>" `
    --set global.telerikEncryptionKey="<INSERT SECRET HERE>" `
    --timeout "30m0s" `
    --wait

# forward port from your host to the CM and then you can browse http://localhost:7777
kubectl port-forward deploy/release1-cm 7777:80 --namespace $namespace
```

## TODO's

- GitHub action for updating repo in GitHub Pages branch.
- Unicorn shared secret should be optional and/or container environment variables customizable.
- Document values: [https://helm.sh/docs/chart_best_practices/values/#document-valuesyaml](https://helm.sh/docs/chart_best_practices/values/#document-valuesyaml)
- Set `required` on required values.
- Add chart meta data such as keywords, sources, maintainers.