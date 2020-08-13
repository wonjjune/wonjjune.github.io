
## metricbeat
* metric 은 타임스탬프와 한두가지 숫자값을 포함하는 이벤트로 로그와는 달리 주기적으로 값을 보내게 된다. 주로 리소스 사용이나 테이터베이스 실행 등 소프트웨어나 하드웨어의 상태 모니터링 맥락에서 사용된다.


## metricbeat - prometheus 대체 및 연동
### prometheus - metricbeat
 * metricbeat 의 prometheus module 을 사용하여 매트릭 데이터를 가져올 수 잇다.
 * prometheus exporter 에서 데이터를 가져오는 방식으로 사용한다.
 * 프로메테우스 서버에서 매트릭 정보를 가져올 때
```
metricbeat.modules:
- module: prometheus
  period: 10s
  hosts: ["localhost:9090"]
  metrics_path: /metrics
```
* exporter 에서 데이터를 가져올 때
```
- module: prometheus
  period: 10s
  hosts: ["node:9100"]
  metrics_path: /metrics
```
* federation api 를 사용하면 프로메테우스 서버의 모든 매트릭 정보를 가져올 수 도 있다.
```
metricbeat.modules:
- module: prometheus
  period: 10s
  hosts: ["localhost:9090"]
  metrics_path: '/federate'
  query:
    'match[]': '{__name__!=""}'
```

# metricbeat 설정
* index name 바꾸기 매우 힘듬
* index lifecycle manager 사용하면 xpack 인지 확인
* ilm 을 default , true 로 두면 기존 index 설정 무시하고 ilm 에 따라 설정함
* 위의 이유로 index 명 변경시 ilm 을 사용하여 변경하여야 함 overwrite 도 해야함