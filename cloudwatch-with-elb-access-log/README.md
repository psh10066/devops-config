# AWS CloudWatch란?

AWS 리소스와 AWS에서 실시간으로 실행중인 Application을 모니터링하는 서비스

## ELB 액세스 로그 모니터링

Amazon Elastic Load Balancer에서 액세스 로그를 수집하여 S3에 저장하고, Lambda를 통해 CloudWatch에서 확인할 수 있다.

## 절차

### 로그 저장

1. 로그를 저장할 S3 버킷 생성
2. 버킷 > 권한 > 버킷 정책 편집 > 정책 기입
   - s3policy.json 내용 편집하여 기입
      - `{elb-account-id}` : 리전의 ELB용 ID (서울 : 600734575887)
      - `{bucket-name}` : S3 버킷 이름
      - `{aws-account-id}` : 내 AWS 계정 ID
3. EC2 > 로드 밸런서 > 로드 밸런서 생성/
   - 로드 밸런서 유형 : Application Load Balancer
4. 로드 밸런서 속성 편집
   1. 로드 밸런서 선택 > 속성 > 편집
   2. 모니터링 > 액세스 로그 활성화, S3 버킷 연동
5. S3 버킷에서 AWSLogs 폴더 > 내 AWS 계정 ID로 된 폴더 확인
   - 시간이 조금 지나면 elasticloadbalancing이라는 폴더가 생성되고, 이 폴더 안에 액세스 로그가 정상적으로 수집된다.

### CloudWatch 연동

1. Lambda 함수 생성
   - 런타임 : Python 3.7 이상
2. Lambda 함수 > 코드 소스 > lambda_handler에 handler.py 파일 내용 복사 붙여넣기
   - `lambda_handler` 메소드 첫 줄 loggroup 변수로 로그 그룹명 설정
3. Lambda 구성 > 권한
   - 만들어진 역할 선택 > 엮여있는 정책 선택 > 편집 > lambda-policy.json 내용 복사 붙여넣기
4. Lambda 트리거 추가
   1. 소스 선택 : S3
   2. Bucket : 위에서 만든 버킷 설정
   3. Event types : All object create events
   4. Prefix : AWSLogs/{aws-account-id}/elasticloadbalancing/{region-name}/
      - `{aws-account-id}` : 내 AWS 계정 ID
      - `{region-name}` : 리전 명칭 (서울 : ap-northeast-2)
5. CloudWatch > 로그 그룹 > 로그 확인
   - 시간이 조금 지나서 트리거가 동작하면 확인할 수 있다.

## 참고 자료

https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html