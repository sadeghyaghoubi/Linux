for chart in *.tgz; do
  curl -u USER:PASS \
    --upload-file "$chart" \
    http://192.168.X.Y:8081/repository/helm-hosted/"$chart"
done
