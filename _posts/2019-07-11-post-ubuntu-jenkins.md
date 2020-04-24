---
title: "Ubuntu에 Jenkins 설치하기"
comment: true
date: 2019-07-10
categories: [CI&amp;CD, Jenkins]
tags: [Jenkins, Ubuntu]
last_modified_at: 2019-07-10T18:45:25-05:00

---

## Ubuntu에 Jenkins 설치하기



오늘은 Jenkins를 설치 해 보도록 하겠습니다.

잠깐 젠킨스의 역사에 대해서 얘기하자면,


> Hudson은 2005년 2월7일 1.0 발표되었는데, 일본 출신의 개발자 Kohsuke Kawaguchi가 Sun에 재직할 당시 개발 되었었다.
(2010/4월 Sun 퇴사) 
>
>그런데, Sun이 Oracle에 인수된 이후에 문제가 생기기 시작했는데, Hudson이 배포되고 있던 java.net의 인프라의 문제로 인해 배포 사이트를 다른 곳으로 
옮기려는 논의가 Hudson 커뮤니티 내부에서 진행이 시작되었고, 그러던 중에 Oracle에서 개발자 메일링 리스트에 Koshuke Kawaguchi를 제외하고 
Oracle 직원으로 그 자리를 대체하면서 논란이 시작 되었다.
>
>그 후 Oracle에서 Hudson에 대한 상표권리를 주장하면서 논란이 정점에 이르더니...
결국 Hudson 커뮤니티에서 Hudson을 Jenkins CI로 개명에 이르게 되었다.

오라클의 횡포 ? ㅎㄷㄷ

자, 설치를 해보도록 합시다.

#### 1. 다운로드
- 젠킨스 홈 https://jenkins.io/index.html 을 각시면 좌측 상단에 Donwload 라고 보기 쉽게 되어있습니다.

![젠킨스 다운로드](/images/post/install_jenkins/install.png)

여기서 릴리즈 유형에 대해서 잠깐 알아 보자면,



>LTS Release는 릴리즈 텀이 긴 안정화 버전이라고 보시면 되고,
>
>Weekly Release인 경우 아주 짧은 릴리즈 기간을 가진 버전이라고 보시면 됩니다.



스탠다드 한 버전을 변경없이 오래 써야 된다면 LTS 버전을 다운로드 하시길 바랍니다.



Ubuntu는 Debian 계열의 리눅스 이므로,



Ubuntu/Debian 을 클릭하시면 다운로드 페이지로 이동합니다.


![Ubuntu/Debian 다운로드](http://pkg.jenkins-ci.org/debian-stable/)




apt-get을 이용해서 설치 합니다.

다음 명령으로 apt-get 을 업데이트 적용


	wget -q -O - http://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

/etc/apt/sources.list  에 적당한 곳에 다음 내용을 추가 합니다 

	sudo vi /etc/apt/sources.list
	deb http://pkg.jenkins.io/debian-stable binary/


다시 업데이트 후 설치를 마무리 합니다. 

	sudo apt-get update
	sudo apt-get install jenkins

![apt-get](/images/post/install_jenkins/apt-get.png)


위 사진처림 설치가 완료되면 자동으로 Jenkins가 실행 됩니다.

다음 명령어로 젠킨스를 제어 할 수 있습니다.

	sudo service jenkins stop
	sudo service jenkins start
	sudo service jenkins restart
    

Jenkins의 웹으로 접근 해봅시다.


Default Port는 8080입니다.

![Default 세팅](/images/post/install_jenkins/connect_jenkins.jpg)


해당 웹으로 진입하면 잠금되어진 화면을 만날 수 있는데 초기화 비밀번호를 넣어야 합니다.

초기화 비밀번호가 있는 위치를 알려주네요.



초기화 비밀번호를 넣고 컨티뉴를 누르면 선택지가 나오는데 기본적인 플러그인을 설치하는 내용입니다.

처음 버튼을 누르고 진행하면 

자동으로 하나씩 설치하기 시작합니다.
설치가 다 되면 계정을 생성하는 화면이 나오는데



계정을 간략하게 생성하고나면 드디어 젠킨스의 메인 화면을 보실 수 있습니다.

![초기화면](/images/post/install_jenkins/jenkins_init.jpg)

이제 빌드 배포 자동화를 시작하면 됩니다.