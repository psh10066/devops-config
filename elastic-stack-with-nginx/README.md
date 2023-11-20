# Elastic Stack이란?

Log Data 수집 및 Process, monitoring을 위한 기술 스택

## Elastic Stack with Nginx
filebeat -> logstash -> elasticsearch -> kibana로 이어지는 모니터링 테스트.

nginx로 접속하는 로그를 모니터링할 수 있다.

## 절차

1. docker, docker compose 설치
2. 터미널에서 아래 명령어 실행
    ```shell
    docker-compose up -d
    ```
3. http://localhost:8080 으로 접속하여 로그를 쌓는다. nginx default 페이지가 보여야 정상.
4. http://localhost:5601 로 접속하여 kibana에 잘 접속되는지 확인한다.
5. 아래 중 하나의 방법으로 확인한다.
   - Menu > Management > Dev Tools에서 _search를 통해 확인
   - Menu > Analytics > Discover에서 data view를 생성하고 index를 연결한 후 확인

## 참고사항

- logstash.conf의 output 블록에서 if문 주석을 해제하면 elasticsearch로 쌓이는 것이 아닌 stdout을 통해 docker log로 확인할 수 있다.