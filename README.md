# p-14653-1-mission

<details>
<summary>1강 Docker Desktop에 Kubernetes 설치</summary>
<div markdown="1">
<img src="image/img.png" width="600" alt="프로젝트 이미지">
</div>
</details>
<br>
<details>
<summary>2강 kubectl 기본 명령어</summary>
<div markdown="2">

# 노드 목록 확인
kubectl get nodes

# 노드 상세 정보
kubectl describe node docker-desktop

# 네임스페이스 목록
kubectl get namespaces

# 또는 줄여서
kubectl get ns

# 특정 네임스페이스의 모든 리소스
kubectl get all -n kube-system
</div>
</details>
<br>
<details>
<summary>3강 첫 번째 Pod 생성하기</summary>
<div markdown="3">

# nginx Pod 생성
kubectl run nginx-pod --image=nginx:latest

# Pod 목록 확인
kubectl get pods

# Pod 상태 확인 (실시간)
kubectl get pods -w

# Pod 상세 정보
kubectl describe pod nginx-pod

# Pod 내부 쉘 접속
kubectl exec -it nginx-pod -- bash

# nginx 설정 파일 확인
cat /etc/nginx/nginx.conf

# 나가기
exit

# Pod 로그 확인
kubectl logs nginx-pod

# Pod 삭제
kubectl delete pod nginx-pod
</div>
</details>
<br>
<details>
<summary>4강 YAML 파일로 Pod 정의하기</summary>
<div markdown="4">
왜 YAML 파일을 사용이유<br>
kubectl run으로 Pod를 만들 수 있지만, 실무에서는 거의 항상 YAML 파일을 사용

# Pod 생성
kubectl apply -f nginx-pod.yaml

# Pod 상태 확인 (-o wide로 더 많은 정보)
kubectl get pods -o wide

# Pod YAML 확인 (Kubernetes가 추가한 정보 포함)
kubectl get pod nginx-pod -o yaml

# Pod 삭제 (파일 기반)
kubectl delete -f nginx-pod.yaml

# 특정 라벨로 Pod 필터링 (Service, Deployment 등이 Pod를 찾을 때 사용) 
kubectl get pods -l app=nginx
kubectl get pods -l environment=dev
</div>
</details>
<br>
<details>
<summary>5강 Multi-Container Pod</summary>
<div markdown="5">
왜 하나의 Pod에 여러 컨테이너를 넣나요?<br>
일반적으로 하나의 Pod에는 하나의 컨테이너를 넣는 것이 권장됩니다. 하지만 밀접하게 협력해야 하는 컨테이너들은 같은 Pod에 넣습니다.

같은 Pod에 넣는 경우:

항상 같이 배포되어야 함

같은 네트워크/스토리지를 공유해야 함

스케일링 단위가 같음
</div>
</details>
<br>
<details>
<summary>6강 Deployment 생성하기</summary>
<div markdown="6">
Deployment를 사용하는 이유:

자동 복구: Pod가 죽으면 새로 만듦

스케일링: Pod 수를 쉽게 늘리고 줄임

롤링 업데이트: 무중단으로 새 버전 배포

롤백: 문제 시 이전 버전으로 복원
</div>
</details>
<br>
<details>
<summary>7강 스케일링 (Scaling)</summary>
<div markdown="7">
스케일링이란?<br>
트래픽이 늘어나면 서버를 더 투입하고, 줄어들면 서버를 줄이는 것을 스케일링이라고 합니다. Kubernetes는 수평 스케일링에 특화되어 있습니다.

수평 스케일링 (Horizontal)	인스턴스 수를 늘림	Pod 3개 → 5개

수직 스케일링 (Vertical)	인스턴스 성능을 높임	CPU 1코어 → 4코어

</div>
</details>
<br>
<details>
<summary>8강 롤링 업데이트 (Rolling Update)</summary>
<div markdown="8">
롤링 업데이트란?<br>
새로운 버전의 앱을 배포할 때, 한 번에 모든 Pod를 교체하면 서비스가 중단됩니다.

작업 1: 이미지 버전 업데이트

작업 2: 롤백 (Rollback)

롤링 업데이트의 장점:

무중단 배포: 항상 일부 Pod가 서비스 중

안전한 배포: 문제 발생 시 롤백 가능

점진적 검증: 새 버전이 안정적인지 확인하면서 배포

</div>
</details>
<br>
<details>
<summary>9강 ClusterIP Service</summary>
<div markdown="9">
왜 Service가 필요한가요?<br>
Pod는 일시적입니다. 삭제되고 다시 만들어지면 IP 주소가 바뀝니다.

Service의 역할:
<br>고정된 접점 제공: Pod가 바뀌어도 Service 이름/IP는 유지
<br>로드 밸런싱: 여러 Pod에 트래픽 분산
<br>서비스 디스커버리: 이름으로 Pod를 찾을 수 있음

Service 테스트

# Deployment가 없다면 먼저 생성
kubectl apply -f nginx-deployment.yaml
kubectl get pods -l app=nginx  # Running 상태 확인

# Service 생성
kubectl apply -f nginx-service-clusterip.yaml


# Service 확인
kubectl get services<br>
kubectl get svc # 또는 줄여서

# Service 상세 정보 (Endpoints 확인)
kubectl describe service nginx-service


# 클러스터 내부에서 테스트 (임시 Pod 사용)
kubectl run curl-test --image=curlimages/curl -it --rm --restart=Never -- curl nginx-service

🔍 명령어 해설:

run curl-test: curl-test라는 이름의 Pod 생성

--image=curlimages/curl: curl이 설치된 이미지 사용

-it: 인터랙티브 + TTY

--rm: 명령 실행 후 Pod 자동 삭제

--restart=Never: Pod가 종료되면 재시작하지 않음 (일회성 실행)

-- curl nginx-service: Pod 안에서 curl 실행

⚠️ --restart=Never 옵션이 중요합니다!
이 옵션이 없으면 Pod가 CrashLoopBackOff 상태가 될 수 있습니다. curl은 한 번 실행하고 종료되는데, Kubernetes가 계속 재시작하려고 하기 때문입니다.

🤔 왜 nginx-service라는 이름으로 접속할 수 있나요?
Kubernetes는 내부 DNS 서버(CoreDNS)를 가지고 있어서, Service 이름을 자동으로 IP로 변환해줍니다.
</div>
</details>
<br>

<details>
<summary>10강 NodePort Service</summary>
<div markdown="10">
작업 1: NodePort Service 생성<br>
작업 2: NodePort로 외부 접근<br>
- Deployment가 없다면 먼저 생성<br>
kubectl apply -f nginx-deployment.yaml
kubectl get pods -l app=nginx  # Running 상태 확인<br>
- Service 생성<br>
kubectl apply -f nginx-service-nodeport.yaml<br>

# 브라우저에서 접근 
http://localhost:30080

# 터미널에서 테스트
curl http://localhost:30080
</div>
</details>
<br>