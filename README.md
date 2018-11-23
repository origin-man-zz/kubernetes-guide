# Kubernetes?


## 용어
# kubernetes-guide in local test

## 1. kubernetes 설치

### Docker for mac 에서 kubernetes 설치 가능
<img src="/images/install_kubernetes_1.png" width="400" height = "200"> <img src="/images/install_kubernetes_2.png" width="400" height = "200"><img src="/images/install_kubernetes_3.png" width="400" height = "200">


## 2. kubeernetes spring-boot app 배포

### image 배포

* $ kubectl run {deployment 이름 지정} --image={docker hub에 업로드된 image 주소}:{image version} --port={포트번호}

### deployment 확인
* $ kubectl get deployments 

### pod 확인
* $ kubectl get pods

#### 이렇게 배포를 했다... 근데 왜 접근이 안되지??
#### kubernetes는 Node가 죽거나 하는 상황에 혹은 트래픽에 대비하여 pod에대한 복사본(replica)를 만든다. 즉 하나의 앱을 배포하면 여러 pod들이 떠있을 수 있다.
#### pod들은 같은 Cluster, 같은 Node에 있다 하더라도 서로다른 내부ip를 가지고 있다. 그렇다면 client는 어느 pod에 접근해야하는가?
#### 이를 중재하는것이 바로 servcie이다. service를 통해서 내부 Cluster를 expose시킴으로 인해 외부에서 Cluster내부의 pod들에 접근 가능하게 해준다.
<img src="/images/pods_and_service_relation.png" width="400" height = "200"> <img src="/images/pods_and_service_relation2.png" width="400" height = "200">

#### service의 종류

* ClusterIP (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
* NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
* LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
* ExternalName - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.
#### 그렇다면 service는 어떻게 만드는가?

### service생성 (expose)
* $ kubectl expose deployment/{delpoyment 이름} --type="NodePort" --port=8080
* $ export NODE_PORT=$(kubectl get services/{service 이름} -o go-template='{{(index .spec.ports 0).nodePort}}')
* $ echo NODE_PORT=$NODE_PORT

port 번호가 보일것이다. "localhost:{포트번호}/"로 접근해보자


