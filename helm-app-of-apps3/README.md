# Helm app-of-apps
Helm template 기반으로 applications를 실행시키고 app-of-apps 패턴을 적용한 예시 입니다.

- template 디렉토리 아래의 `Application CRD` 리소스들을 `root application` 이라고 불리는 어플리케이션으로 헬름을 이용해 묶어서 사용합니다.

- Argo cd에서 현재 디렉토리(helm-app-of-apps)를 Path로 사용하여 Chart.yaml 에 정의된 `helm-app-of-apps-example` 이름의 어플리케이션을 등록하면 하위에 localstack, redis가 켜지는 예제입니다.

### 구조
```
.
├── Chart.yaml              //root applications charts
├── README.md
├── templates
│   ├── localstack.yaml
│   └── redis.yaml
└── values                  //root applications values
    ├── prod-values.yaml
    └── stage-values.yaml
```

### register application
```
$ kubectl apply -n argo-system -f ./application.yaml
```

#### application.yaml
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: helm-app-of-apps
  namespace: argo-system
spec:
  project: default
  source:
    repoURL: https://github.com/modusign/applications.git
    targetRevision: HEAD
    path: example/helm-app-of-apps
  destination:
    server: https://kubernetes.default.svc
    namespace: argo-system
```