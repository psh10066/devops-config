# Prometheus and Grafana with Spring Actuator

Spring 운영 환경을 Prometheus와 Grafana를 통해 모니터링할 수 있다.

- Spring Actuator : 지표(metric), 추적(trace), 감사(auditing) 등 다양한 편의 기능을 제공하는 스프링 부트 라이브러리
- Prometheus : 시스템 이벤트 모니터링 및 경고를 제공하는 시계열 오픈소스 모니터링 도구
- Grafana : 다양한 데이터 소스에 대한 대시보드를 제공해 주는 오픈소스 Data Visualization 도구

## 절차

1. Spring Boot Application의 build.gradle 파일에 아래 내용 추가
   ```groovy
   implementation 'org.springframework.boot:spring-boot-starter-actuator'
   implementation 'io.micrometer:micrometer-registry-prometheus'
   ```
2. Spring Boot Application의 application.yml 파일에 아래 내용 추가
   ```yaml
   management:
    server:
      port: 9292 # actuator만 포트 변경
    endpoints:
      web:
        exposure:
          include: prometheus
   ```
3. docker, docker compose 설치
4. prometheus.yml의 spring-actuator targets 부분에 실행되고 있는 Spring Boot Application의 Actuator 도메인 입력
   - 우리는 Prometheus를 Docker로 실행하기 때문에 만약 Application을 로컬에서 실행하고 있는 경우 `localhost:9292`가 아닌 아래와 같이 입력해 주어야 한다.
      ```
      host.docker.internal:9292
      ```
5. 터미널에서 아래 명령어 실행
    ```shell
    docker-compose up -d
    ```
6. http://localhost:3000 으로 접속하여 Grafana에 로그인한다.
   - 초기 비밀번호 : admin / admin
7. Menu > Dashboards > Create Dashboard > Import dashboard를 통해 외부 대시보드 템플릿을 불러온다.
   - https://grafana.com/grafana/dashboards/ 에서 spring으로 검색 후 Spring Boot에 맞는 대시보드를 고른다.
      - 가장 보편적인 템플릿 : https://grafana.com/grafana/dashboards/4701-jvm-micrometer/ (4701)
   - 고른 대시보드의 ID를 입력하고 Load
   - Prometheus data source 선택 후 Import
8. 대시보드 확인