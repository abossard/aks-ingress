openssl req -x509 -sha256 -newkey rsa:4096 -keyout ca.key -out ca.crt -days 356 -nodes -subj '/CN=My Cert Authority'
openssl req -new -newkey rsa:4096 -keyout server.key -out server.csr -nodes -subj '/CN=mydomain.com'
openssl x509 -req -sha256 -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt
openssl req -new -newkey rsa:4096 -keyout client.key -out client.csr -nodes -subj '/CN=My Client'
openssl x509 -req -sha256 -days 365 -in client.csr -CA ca.crt -CAkey ca.key -set_serial 02 -out client.crt

kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/docs/examples/http-svc.yaml
kubectl apply -f .

curl -k --resolve mydomain.com:443:51.124.145.169 https://mydomain.com/
curl -k -E client.crt --key client.key --cacert ca.crt --resolve mydomain.com:443:51.124.145.169 https://mydomain.com/