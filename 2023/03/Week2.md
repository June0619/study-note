# Week 2 (2023-03-13 ~ 2023-03-19)

## RabbitMQ
### Introduce
- Protocol : AMQP
- 범용 병렬 함수형 프로그래밍 언어 `Erlang` 으로 작성
- 관용적으로 Publisher 와 Consumer 로 분리
- 통신 간 모델 정의가 문제가 있는 듯

### Consumer
- `@RabbitListener` 사용 (queue 지정 필요)
- 