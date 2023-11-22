# AWS CloudWatch란?

AWS 리소스와 AWS에서 실시간으로 실행중인 Application을 모니터링하는 서비스

## EC2 모니터링

운영 체제 전반에 걸쳐 Amazon EC2 인스턴스에서 내부 시스템 수준 지표를 수집할 수 있다.

## 절차

1. IAM > 역할 > 역할 생성
   1. 신뢰할 수 있는 엔터티 유형 : AWS 서비스
   2. 서비스 또는 사용 사례 : EC2, 사용 사례 : EC2
   3. 권한 정책 : CloudWatchAgentServerPolicy
2. EC2 인스턴스에 만든 IAM 적용
3. 서버 접속 후 CloudWatch 에이전트 설치
   - 각 OS마다 설치 명령어가 다르다.
   - https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-commandline-fleet.html
4. amazon-cloudwatch-agent.json 파일 추가
   - 파일 경로 : /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
5. CloudWatch 시작
   ```shell
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
   ```

## 참고사항

- 예시 amazon-cloudwatch-agent.json에 nginx 관련된 내용이 존재하므로 nginx를 사용하지 않는 경우에는 제거가 필요할 수 있다.

## 참고 자료

https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html