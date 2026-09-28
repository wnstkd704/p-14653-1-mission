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