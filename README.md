### 프론트엔드 배포 파이프라인

개요
![image](https://github.com/user-attachments/assets/e46b8a7f-cd87-4189-84f8-8184b5a5c95e)

## 주요 링크

- S3 버킷 웹사이트 엔드포인트 : [http://jdj-aws-bucket.s3-website-ap-southeast-2.amazonaws.com/](http://bucket-nahee.s3-website-ap-northeast-1.amazonaws.com)
- CloudFront 배포 도메인 이름 : [https://d2hnnj815xakwr.cloudfront.net](https://d2xkqga3hcd31j.cloudfront.net)

## 주요 개념

1. GitHub Actions과 CI/CD 도구

#### GitHub Actions

- GitHub 저장소에서 직접 워크플로우를 자동화할 수 있도록 지원하는 CI/CD 도구
- 코드 푸시, PR 생성, 이슈 발생 등의 이벤트에 따라 자동으로 스크립트 실행가능.
- GitHub Actions에서 사용하는 주요 개념
- Workflow: 자동화된 프로세스를 정의하는 파일 (.github/workflows/\*.yml)
- Event: 워크플로우를 트리거하는 이벤트 (예: push, pull_request, schedule)
- Job: 개별 작업 단위, 하나의 워크플로우는 여러 개의 Job을 포함할 수 있음.
- Step: Job 내부에서 실행되는 개별 명령어 또는 액션.
- Runner: GitHub에서 제공하는 실행 환경 (예: ubuntu-latest, macos-latest)
- Actions: 재사용 가능한 작업 단위 (예: actions/checkout@v3 → 저장소 체크아웃)
- Secrets: 보안 키(환경 변수) 저장 공간 (예: AWS IAM Access Key, API Key)

#### CI/CD도구

- 코드 변경을 자동으로 빌드, 테스트, 배포하는 프로세스를 지원하는 도구
- Continuous Integration (CI), Continuous Delivery (CD), Continuous Deployment (CD) 세 가지로 이루어짐.

- CI(Continuous Integration)
- 코드 변경 사항을 병합하고, 자동으로 빌드와 테스트를 수행하는 과정
- CD(Continuous Delivery)
  - CI가 완료된 후 자동으로 준비된 상태로 배포 가능하도록 설정하는 과정
  - 배포할 빌드 파일을 생성
  - 스테이징 환경에 배포해 검토
- CD (Continuous Deployment, 지속적 배포)
  - 코드 변경 사항을 자동으로 운영 환경(Production)에 배포하는 과정

2. S3와 스토리지

#### S3

- AWS에서 제공하는 객체 기반 스토리지 서비스
- Bucket
  - S3에서 데이터를 저장하는 컨테이너로, 모든 객체는 버킷 내에 저장함
- Object
  - 버킷에 저장되는 파일과 그 파일에 대한 메타데이터로 구성됨
- Key
  - 객체를 식별하는 고유한 이름으로, 버킷 내에서 객체를 찾는 역할
- Region
  - 버킷과 객체가 물리적으로 저장되는 AWS 데이터 센터의 위치

#### 스토리지

- 데이터를 저장하고 관리하는 시스템, 기술
- 스토리지 유형
- 객체 스토리지
  - 데이터를 객체 단위로 저장하며, 메타데이터와 함께 저장
  - 확장성이 뛰어나고 대용량 데이터를 저장하는데 적합
  - S3, Google Cloud Storage 등
- 블록 스토리지
  - 데이터를 블록 단위로 저장하며, 일반적으로 운영 체제의 파일 시스템처럼 사용
  - 고성능, 저지연 I/O가 필요한 애플리케이션에 적합
  - AWS의 EBS(Elastic Block Storage)
- 파일 스토리지
  - 파일 및 폴더 구조로 데이터를 관리하며, 네트워크 파일 시스템(NFS)형태로 제공
  - 여러 서버가 동시에 접근할 수 있는 공유 스토리지로 활용
  - AWS의 EFS(Elastic File System)

3. CloudFront와 CDN

#### CloudFront

- Amazon CloudFront는 AWS 인프라와 긴밀하게 통합되어 있으며 전 세계 엣지 로케이션을 통해 콘텐츠를 안정적이고 빠르게 전달하는 AWS에서 제공하는 CDN 서비스
- CloudFront는 기본적으로 HTTPS를 지원하며, 커스텀 SSL 인증서,AWS WAF 등을 통해 보안을 강화할 수 있다.
- Lambda@Edge를 통해 엣지 로케이션에서 사용자 정의 코드를 실행할 수 있다. 이를 통해 요청 및 응답을 실시간으로 변경하거나 추가적인 처리 가능.

#### CDN (Content Delivery Network)

전 세계에 분산된 서버 네트워크를 통해 웹 콘텐츠를 사용자에게 빠르게 전달하는 기술

- 엣지 로케이션 (Edge Locations):
  사용자와 가까운 위치에 있는 서버로, 콘텐츠를 캐시하여 지연 시간을 줄이고 빠른 응답을 제공.

- 캐싱 (Caching):
  자주 요청되는 콘텐츠(이미지, 동영상, HTML 등)를 임시로 저장해, 원본 서버에 대한 부담을 줄이고 응답 속도를 향상.

- 오리진 서버 (Origin Server):
  CDN이 캐시하지 않은 콘텐츠를 요청할 때, 원본 데이터를 제공하는 서버(예: 웹 서버, S3 버킷 등).

- 로드 밸런싱 (Load Balancing):
  전 세계의 분산된 서버로 사용자 요청을 효율적으로 분산시켜, 안정적인 서비스 제공.

- DNS 기반 라우팅:
  사용자의 요청을 가장 가까운 엣지 로케이션으로 라우팅해, 최적의 전송 경로를 선택함.

4. 캐시 무효화(Cache Invalidation):
   CDN이나 웹 캐시 시스템에 저장된 이전 버전의 데이터를 제거하거나 만료시켜, 사용자가 최신의 콘텐츠를 받을 수 있도록 하는 과정

- 무효화 방법

  - 수동 무효화: 관리자가 특정 파일이나 경로에 대해 직접 무효화 요청을 실행하여 캐시를 삭제.

  - 자동 무효화: 미리 설정된 만료 시간(TTL, Time To Live)이 경과하면 캐시된 데이터가 자동으로 만료되어 새롭게 갱신.

- CloudFront에서의 활용: AWS CloudFront에서는 무효화 요청을 통해 지정한 경로의 캐시를 삭제할 수 있다. 무효화 요청이 처리되면, 이후 사용자의 요청 시 오리진 서버에서 최신 콘텐츠를 다시 가져옴.

5.  Repository secret과 환경변수

#### Repository Secret

- GitHub 저장소 설정에서 관리되는 민감한 정보를 안전하게 저장하는 기능.
- API 키, 토큰, 비밀번호 등 노출되면 안 되는 민감한 정보를 코드에 직접 포함시키지 않고, 암호화된 형태로 저장하여 보안을 강화한다.
- 사용 방법: 워크플로우 파일 내에서 ${{ secrets.SECRET_NAME }} 형태로 참조하여 사용 가능.

#### 환경변수 (Environment Variables)

애플리케이션이나 스크립트가 실행되는 환경에서 동적으로 값을 설정하고 사용할 수 있는 변수.
빌드 설정, 배포 대상, API 엔드포인트 등 다양한 설정 값을 코드 외부에서 관리할 수 있게 도와준다.

#### 특징

- 유연성:
  - 워크플로우 파일 내에서, 특정 Job이나 Step 수준에서 변수로 정의하여 사용 가능.
  - 시스템 환경변수로서 기본적으로 제공되는 값들이 있으며, 사용자 정의 변수도 설정 가능.
- 보안성: 민감하지 않은 설정 값들은 일반 환경변수로 관리, 민감한 정보는 Repository Secret을 활용하여 관리하는 것이 좋음.
- 설정 방법: YAML 파일 내에서 env 키워드를 사용하여 변수 값을 설정 가능.

## CDN과 성능 최적화

#### 테스트 환경
- S3 버킷 웹사이트 엔드포인트 : [http://jdj-aws-bucket.s3-website-ap-southeast-2.amazonaws.com/](http://bucket-nahee.s3-website-ap-northeast-1.amazonaws.com)
- CloudFront 배포 도메인 이름 : [https://d2hnnj815xakwr.cloudfront.net](https://d2xkqga3hcd31j.cloudfront.net)

CloudFront에 요청한 컨텐츠는 X-Cache: Hit from cloudfront 라는 헤더가 붙는다

|분류|S3|CloduFront|
|----|----|----|
|X-Cache 유무|<img width="1440" alt="image" src="https://github.com/user-attachments/assets/052f41a8-c0a1-4e39-a957-afe78f9efd19" />|<img width="1440" alt="image" src="https://github.com/user-attachments/assets/8a6b3a85-5fcb-405d-89a1-774346e8cd0e" />|
|size & time 비교|<img width="1440" alt="image" src="https://github.com/user-attachments/assets/5f8b9cf7-8996-4087-bf34-5eacd7afcc1a" />|<img width="1440" alt="image" src="https://github.com/user-attachments/assets/658da0d6-d144-45ee-ac66-d34621a51923" />|




