# nGrinder 부하테스트

* ngrok 으로 부하테스트 하기
  https://600a-218-48-199-126.ngrok-free.app

![ngrok2.png](ngrok2.png)
* quick start에 요청할 url을 입력하면 간단히 테스트가 만들어진다
* 자동으로 script 만들어줘서 만질 일은 없었다
  * 더 세세한 script를 만들어야 된다면 수정해야된다

* 1분에 100명정도 오도록 performance test 작성하기
  * Agent 1
  * Vuser per agent 99
  * Duration 1min
  * Enable Ramp-Up 을 설정하면 서서히 증가하게 된다

![ngrok3.png](ngrok3.png)

* Total Vusers: 테스트에 참여한 총 사용자 수.
    * 총 99명의 사용자가 테스트에 참여

* TPS (Transactions Per Second): 초당 처리된 트랜잭션 수
    * Peak TPS: 테스트 도중 기록된 최대 TPS 값
    * 평균 TPS는 2.7이며, 최대 TPS는 8.0이다

* Mean Test Time: 테스트를 수행하는데 걸린 평균 시간
  * 평균 시간은 약 21,519.28 밀리초(약 21.5초)

* Executed Tests: 실행된 총 테스트 수 
  * 총 105번의 테스트가 실행됨

* Successful Tests: 성공적으로 완료된 테스트 수
  * 88번의 테스트가 성공함

* Errors: 테스트 도중 발생한 오류 수
  * 17개의 오류가 발생함

* Run time: 테스트를 실행한 총 시간
  * 1분 동안 테스트가 수행됨