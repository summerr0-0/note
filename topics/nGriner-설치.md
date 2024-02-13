# nGriner 설치

* docker ngrainder 설치
* controller와 agent를 설치해야 되며 간단한 테스트가 아니라면 다른 서버에 설치하는 것을 권장한다
  * 여기서는 한 서버에 설치

1. controller 설치
```Bash
docker pull ngrinder/controller

docker run -d -v ~/ngrinder-controller:/opt/ngrinder-controller --name controller -p 80:80 -p 16001:16001 -p 12000-12009:12000-12009 ngrinder/controller
```

2. agent 설치
```Bash
docker pull ngrinder/agent

docker run -d -v ~/ngrinder-agent:/opt/ngrinder-agent --name agent ngrinder/agent controller_ip:controller_web_port
```

* `controller_ip`와 `controller_web_port` 지정해준다

* agent와 controller는 다른 서버가 권장되지만 한 컴퓨터에 설치할땐
```Bash
docker run -d --name agent --link controller:controller ngrinder/agent
```

3. 실행 확인
* `http://localhost:80/login` 에 접속
* User ID와 Password 둘 다 admin이다
* 우측 상단 `admin` > `에이전트 관리` 에서 Agent 목록에서 잘 연결되었는지 확인 
![ngrinder설치.png](ngrinder설치.png)
