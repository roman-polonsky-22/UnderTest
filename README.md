# UnderTest

wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

k3d cluster create dev

helm install dev oci://registry-1.docker.io/bitnamicharts/minio

export ROOT_USER=$(kubectl get secret --namespace default dev-minio -o jsonpath="{.data.root-user}" | base64 -d)

export ROOT_PASSWORD=$(kubectl get secret --namespace default dev-minio -o jsonpath="{.data.root-password}" | base64 -d)

kubectl run --namespace default dev-minio-client \
     --rm --tty -i --restart='Never' \
     --env MINIO_SERVER_ROOT_USER=$ROOT_USER \
     --env MINIO_SERVER_ROOT_PASSWORD=$ROOT_PASSWORD \
     --env MINIO_SERVER_HOST=dev-minio \
     --image docker.io/bitnami/minio-client:2023.4.12-debian-11-r4 -- admin info minio
     
echo "MinIO&reg; web URL: http://127.0.0.1:9001/minio"
kubectl port-forward --namespace default svc/dev-minio 9001:9001

Go to browser localhost:9001

helm repo add jenkins https://charts.jenkins.io
helm repo update

helm search repo jenkins

helm install dev jenkins/jenkins

1. Get your 'admin' user password by running:
  kubectl exec --namespace default -it svc/dev1-jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  echo http://127.0.0.1:8080
  kubectl --namespace default port-forward svc/dev1-jenkins 8080:8080
  browser localhost:8080 to navigate to Jenkins
