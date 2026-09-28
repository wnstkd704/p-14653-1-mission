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