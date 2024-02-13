# nGrinder 구조

* Controller와 Agent로 구성되어 있다

* Controller
    * 성능 테스트를 위한 웹 인터페이스를 제공
    * 테스트 프로세스를 조정
    * 테스트 통계를 수집하고 표시
    * 사용자가 스크립트를 생성하고 수정할 수 있는 기능 지원

* Agent
    * Agent Mode가 실행될 때 Target 서버에 프로세스 및 스레드를 실행 시켜 부하 발생
    * Moniter 모드에서 실행 시 대상 시스템 성능 (예 : CPU / 메모리) 모니터링


![nGringer2.png](nGringer2.png)
* Test Name: 테스트의 이름을 설정합니다. 이 이름은 테스트 실행 후 결과를 식별하는 데 사용됩니다.
* Description: 테스트에 대한 설명을 추가합니다. 이 설명은 테스트의 목적이나 특별한 설정 등에 대한 정보를 포함할 수 있습니다.
* Agent: 테스트를 실행할 에이전트의 수를 설정합니다. "Max"는 최대 사용할 수 있는 에이전트의 수를 의미합니다.
* User per agent: 각 에이전트가 시뮬레이션할 가상 사용자(VUser)의 수를 설정합니다. "Max"는 한 에이전트 당 최대 사용자 수를 의미합니다.
* Processes: 각 에이전트가 사용할 프로세스 수를 설정합니다. 프로세스는 동시 실행 환경을 제공합니다.
* Threads: 각 프로세스 내에서 실행될 스레드의 수를 설정합니다. 스레드는 실제 테스트를 수행하는 단위입니다.
* Script: 테스트에 사용할 스크립트를 선택합니다. "svn"과 같은 옵션은 스크립트의 소스가 Subversion 같은 버전 관리 시스템에서 관리되고 있음을 나타냅니다.
* Script Resources: 스크립트 실행에 필요한 추가 리소스(파일, 데이터 등)를 지정합니다.
* Target Host: 테스트 대상의 호스트 주소(URL)를 지정합니다.
* Duration: 테스트를 실행할 시간을 설정합니다. 여기서는 시간, 분, 초 단위로 설정할 수 있습니다.
* Run Count: 테스트를 몇 번 실행할 것인지 횟수를 지정합니다. "Max"는 최대 실행 횟수를 의미합니다.
* Enable Ramp-Up: Ramp-Up 기능을 활성화할지 여부를 결정합니다. Ramp-Up은 테스트 시작 시 사용자를 점진적으로 증가시키는 방법입니다.
* Initial Count: Ramp-Up을 시작할 때의 초기 사용자 수입니다.
* Incremental Step: Ramp-Up 단계마다 증가시킬 사용자 수를 설정합니다.
* Initial Sleep Time: 첫 번째 사용자가 시작하기 전에 대기할 시간을 설정합니다.
* Interval: 사용자를 증가시킬 시간 간격을 설정합니다

![ngrok3.png](ngrok3.png)

* Total Vusers: 테스트에 참여한 총 사용자 수.
* TPS (Transactions Per Second): 초당 처리된 트랜잭션 수
* Peak TPS: 테스트 도중 기록된 최대 TPS 값
* Mean Test Time: 테스트를 수행하는데 걸린 평균 시간
* Executed Tests: 실행된 총 테스트 수
* Successful Tests: 성공적으로 완료된 테스트 수
* Errors: 테스트 도중 발생한 오류 수
* Run time: 테스트를 실행한 총 시간