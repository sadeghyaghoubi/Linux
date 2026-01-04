
```
helm repo add nexus-helm \
  http://192.168.59.29:8081/repository/helm/ \
  --username USER \
  --password PASS
```
```
helm search repo nexus-helm
```
```
mkdir nexus-helm-backup
cd nexus-helm-backup
```
```
for chart in $(helm search repo nexus-helm -o json | jq -r '.[].name'); do
  helm pull "$chart" --destination .
done
```
