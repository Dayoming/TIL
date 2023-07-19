# 유연하고 안전하게 배포 Pipeline 운영하기

### 토스의 Server Platform Team

1. 생산성
2. 안정성
3. 보안, Compliance
4. 서비스, 라이브러리 구현
5. 서비스 플랫폼 운영(Kubernetes, 모니터링 시스템, 빌드, 배포 시스템)

# Pipeline

<aside>
💡 반복하는 일을 자동화하는 시스템

</aside>

- 서비스 빌드 및 배포
- 라이브러리 업로드
- 작업 자동화 등

## Pipeline 운영

- 생산성
    - Pipeline을 빠르게 만들고 수정한다
- 안정성
    - 예상대로 동작한다
- 보안·Compliance
    - 모든 Pipeline에서 반드시 지켜야하는 요건이 있다
    - 서비스를 배포할 때 상호 검증을 하는 등의 절차를 준수(전자금융감독규정 등)
    - 반드시 적용되어 있음을 보장

## Pipeline 운영 전략

**Pipeline 설정 중앙화 →** 여러 스쿼드가 사용하는 Pipeline의 설정을 한 곳에 모아두고 DevOps Engineer가 주도적으로 운영

## GoCD (Go Continuous Delivery)

서버에서 Pipeline을 정의하고 에이전트에서 실행, Jenkins와 비슷한 자동화 시스템

웹 UI에서 편리하게 여러 파이프라인을 실행하고 결과를 확인

Pipeline의 개수가 많아지며 어려움을 겪음

### Challenge 1. 가시성

1. Pipeline이 많으면 웹 UI에서는 어떤 Pipeline이 있는지 확인하는 것부터 쉽지 않음
2. Pipeline 설정을 한 번에 보기 어려움
3. Pipeline 설정이 어떻게 변했는지 알기 어려움

**Pipeline as Code**

Pipeline을 코드로 표현하여 Git에 저장

`gocd-yaml-config-plugin`을 활용

Pipeline 설정을 yaml 파일로 작성하여 Git에 저장하고 GoCD 서버와 연동

![Alt text](./img/pipeline-image1.png)

- 모든 Pipeline 설정이 파일로 저장되어 있어 어떤 Pipeline이 있는지 Git 저장소에서 한 눈에 확인할 수 있고, 여러 단계로 이루어진 Pipeline 설정도 한 파일에서 모두 확인 가능
- Pipeline 설정 파일이 Git 저장소에서 Version Control 되기 때문에 언제 어떻게 변경되었는지 확인 가능

### Challenge 2. 생산성

- 토스뱅크는 Micro Service Architecture를 적극적으로 활용해 새로운 Pipeline을 자주 만든다.
    - 그래서 Pipeline을 빠르고 정확하게 만드는 것이 중요
    - 새로운 Pipeline을 만들 때 같은 내용을 복붙하는 경우가 많음 → 실수 발생 가능
    - 공통 변경 사항이 있을 때 모든 파일을 수정해야 함

**GoCD Template**

비슷한 설정을 Template으로 만들어 여러 Pipeline에서 공유

`gocd-yaml-config-plugin`과 함께 사용 가능

공통적인 설정은 Template을 지정하는 한 줄로 대체되어 훨씬 간결해지고 달라야 하는 설정은 변수로 주입 → 실수의 여지 줄어듬

### Challenge 3. 확장성

변경에 유연하게 대처할 수 있는 것

1. Pipeline 설정은 Git 저장소에서, Template 설정은 Web UI에서 봐야 하므로 둘을 비교하면서 Template을 수정하는 것이 불편
2. Template을 사용하는 모든 Pipeline에 한번에 적용되어 실수의 영향이 큼

**Helm Template**

Template Rendering 도구

- 공통적인 부분은 Template으로 만들고, 고유한 부분을 Values에 정의하여 Rendering 가능
- 조건문, 반복문, 변수 지원 → 복잡한 내용 표현 가능
- YAML 파일 지원
- GoCD 템플릿을 대체하고 더 추상적인 부분도 Template로 만들어 사용 가능
- Helm Template을 Pipeline 설정과 동일한 Git 저장소에 두면 Pipeline 설정과 Template 설정을 한번에 더 쉽게 수정할 수 있다.

![Alt text](./img/pipeline-image2.png)

- 조건문을 이용하면 일부 Pipeline에서만 테스트해볼 수도 있다.
    - 테스트용 Pipeline에서 충분히 동작을 검증하고 전체 Pipeline에 적용하면서 Template 변경 실수가 전체 Pipeline에 영향을 주는 일이 줄어듬
- 호환성 면에서도 이점
    - GoCD Template은 GoCD에서만 사용할 수 있었으나 Helm Template의 Values 파일은 GoCD와 무관한 추상적인 설정
    - 다른 Pipeline 도구를 사용할 때도 그 도구에 맞도록 Template만 새로 작성한다면 Values 파일 재사용 가능

### Challenge 4. 복잡성

시간이 지나며 Template는 점점 복잡해짐 → 의도하지 않은 변경 만들기 쉬움

**CI (Continuous Intergration)**

검증을 통과한 변경 사항만 반영(GitHub Actions 사용)

1. Template 변경 사항을 작업하는 브랜치를 분리
2. 검증을 통과했을 때에만 master 브랜치에 반영
    - 검증은 develop 브랜치에 변경 사항이 Push 되면 모든 Pipeline 설정 파일 다시 Randering
    - 의도한 변경은 이전에 Rendering한 Pipeline 설정 파일에 반영
    - 반면 다시 Rendering 할 때 생기는 차이는 의도하지 않았을 확률이 높음

**Kubernetes Object CI**

토스뱅크의 채널계 서비스는 Kubernetes에서 운영

서비스 배포 시 Helm Template을 사용해 Kubernetes Object 배포

1. Template 변경 사항을 작업하는 브랜치를 분리
2. 검증을 통과했을 때에만 master 브랜치에 반영
    - Kubernetes Object는 Pipeline 설정과 다른 점 → Rendering된 결과 파일을 Kubernetes에 배포할 뿐 Git에 저장하지 않음
    - 변경 전후에 비교할 대상이 없기 때문에 검증 절차 만들기 어려움
    - 따라서, Kubernetes에 배포할 때와 같은 방식으로 Rendering한 Kubernetes Object 파일을 검증용으로 Git에 저장
    - CI를 수행할 때 모든 검증용 Kubernetes Object 파일을 다시 Rendering

Pipeline 설정 파일과 검증용 Kubernetes Object 파일에 차이가 생기지 않는다면 master 브랜치에 반영

# Wrap-up

## 문제 상황

1. 토스뱅크의 Pipeline은 400개가 넘고 매년 두 배로 늘어난다.
2. Pipeline 설정이 복잡하고 자주 변한다.

## Challenges

1. **가시성** → gocd-yaml-config-plugin
2. **생산성** → GoCD Pipeline Template
3. **확장성** → Helm Template
4. **복잡성** → CI

![Alt text](./img/pipeline-image3.png)

이 방법들은 GoCD가 아니더라도 Pipeline as Code를 지원하는 도구라면 적용 가능

Jenkins, GitHub Actions, Spinnaker …

Pipeline as Code는 일반적으로 Version-Control이 되는 Pipeline을 뜻하는 경우가 많음

(Pipeline 설정을 파일 형태로 저장하고 Git에서 확인하는 것까지)

여기에 Helm Template으로 Programmable한 특성을, CI로 Testable한 특성을 더하면 유연하고 안전하게 파이프라인 운영 가능