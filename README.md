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
