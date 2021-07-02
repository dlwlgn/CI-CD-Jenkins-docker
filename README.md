# CI-CD-Jenkins-docker

## Docker 설치
https://docs.docker.com/get-docker

![image](https://user-images.githubusercontent.com/50227342/124048104-8cd5e900-da50-11eb-8d3e-775217c1668c.png)

## Docker로 Jenkins를 설치 및 설정
다음 명령어로 설치
```
docker run -d -u root -p 9090:8080 --name=jenkins jenkins/jenkins
```
http://localhost:9090 을 접속하면 다음과 같이 나온다

![image](https://user-images.githubusercontent.com/50227342/124048425-4b920900-da51-11eb-94c0-951192f6a8e6.png)

다음 명령어를 입력하여 비밀번호를 확인할 수 있다.
```
docker logs jenkins
```
기본 설치 플러그인을 설치해준다.

![image](https://user-images.githubusercontent.com/50227342/124048593-a1ff4780-da51-11eb-86bd-90de094974f6.png)

어드민 유저 정보를 설정해준다.

![image](https://user-images.githubusercontent.com/50227342/124049234-fce56e80-da52-11eb-9b3e-763e2ba6d5f8.png)

설치 확인

![image](https://user-images.githubusercontent.com/50227342/124049341-3322ee00-da53-11eb-94df-88b8f525d02c.png)

## Jenkins와 Gitlab 연동

gitlab 레포지토리에 access token을 발행해야 한다.

gitlab > Settings > Access Tokens로 접근

name과 만료 일자를 설정하고 api를 체크해서 access token을 생성해준다.

![image](https://user-images.githubusercontent.com/50227342/124051123-d9242780-da56-11eb-9329-74e32be0bdb2.png)

그 후, 해당 화면에서 access token을 잘 복사하고 저장해둔다.

![image](https://user-images.githubusercontent.com/50227342/124051171-f658f600-da56-11eb-912e-ee201563ea3c.png)

그 후, Jenkins의 DashBoard > Manager Jenkins > Plugin Manager 플러그인을 설치해준다.

![image](https://user-images.githubusercontent.com/50227342/124052819-1dfd8d80-da5a-11eb-811f-ed6e10448176.png)

그리고 Jenkins와 Gitlab을 연동해보자. Dashboard > Manager Jenkins > configure System 에서할 수 있다.

![image](https://user-images.githubusercontent.com/50227342/124056691-25746500-da61-11eb-967c-3dc241d8872a.png)

Connetcion name에 아무 이름이나 쓰고, URL에 깃랩 주소를 적어주며, Credentials에 add를 눌러 Jenkins를 추가해준다.

설정에서 종류는 GitLab API token으로 설정해주고, API token에서 아까 복사한 토큰 값을 넣어준다. ID는 깃랩의 아이디를 써주고 add를 하여 생성한다.

그리고 오른쪽에 Test Connection을 클릭해서 Success가 나오면 성공이다. 적용 후 저장을 눌러서 나온다.

## Nginx 설치 후 설정

docker로 nginx 정적 웹서버를 설치해보자.

배포될 폴더 dist를 생성해준 뒤, Nginx를 설치한다.
```
mkdir dist
docker run --name nginx -d -p 80:80 -v c:/Users/dlwlgns/dist:/usr/share/nginx/html nginx
```
설치를 마쳤으면 dist 폴더에 간단한 index.html 파일을 생성해서 잘 작동하는지 확인한다.

![image](https://user-images.githubusercontent.com/50227342/124064734-54460780-da70-11eb-9a08-79eea957ddde.png)

## Jenkins pipeline을 이용해서 gitlab 프로젝트 자동 빌드

http://localhost:9090/view/all/newJob 로 접속하여 pipeline 이름을 적당히 넣어준다.

pipeline을 선택하고 ok를 누른다.

gitlab에서 jenkins에 트리거를 전송하기 위해 jenkins에서 제공하는 secret token이 필요하다. 이 코드는 생성된 작업 아이템 설정에서 Build Trigger 섹션 중 Build when a change is pushed to GitLab. GitLab webhook URL: ~~~ 를 클릭하여 고급 버튼을 누르면 생성할 수 있다.

복사를 하고, 저장을 누르고 나왔다면, 이제 소스코드가 있는 깃랩에서 push 이벤트에 대한 trigger를 만들어줄 차례이다. 먼저 깃랩에 settings > integration으로 들어간다.(프로필의 settings가 아니다. 좌측 탭에서 settings임을 주의하자.)

여기서 URL칸에는  Build when a change is pushed to GitLab. GitLab webhook URL: 뒷 부분의 URL을 적어야하는데, localhost 관련으로 block이 되어버리는 문제가 있다. 그러므로 ngrok을 깔아서 포워딩 시킨 주소를 사용해야 한다.

https://ngrok.com/download 에 들어가서 다운받아 실행한 후, ngrok http 9090을 입력하면 포워딩 주소가 나오고 이를 그대로 사용하는 것이 아니라 http://[ngrok에서 얻은 로컬호스트 주소]/project/[생성한 파이프라인 프로젝트명]/ 과 같이 사용해주어야 한다.

그리고 시크릿 토큰은 아까 얻은 그 시크릿 토큰을 입력해주고, 브랜치는 master를 입력한다.

그리고 그 후 add webhook를 클릭해 설정을 저장하고 test 버튼을 통해 테스트하여 파란색 메시지로 HTTP 200이 나오면 성공이다.

그렇다면 이제 파이프라인을 작성할 시간이다. jenkins에서 아까 만든 아이템 pipeline_test에서 구성 > pipeline 탭을 클릭한다.

다음은 pipeline 스크립트의 샘플 구조이다.

![image](https://user-images.githubusercontent.com/50227342/124072174-624d5580-da7b-11eb-8b6b-2876b1f7e518.png)

스크립트 문법에 대해 참고할 만한 사이트

https://waspro.tistory.com/554

https://tjdrnr05571.tistory.com/13

pipeline syntax에 들어가서 문법들을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/50227342/124086432-0212df80-da8c-11eb-8a57-61496a65c2ee.png)

git pull은 
```
git credentialsId: '~~~~~~~~~~', url: 'https://lab.ssafy.com/dlwlgns102/vue-project.git'
```
와 같이 생성이 되는데, 리포 URL과 위에서 만들었던 Credentials 를 넣어주면 코드를 생성할 수 있다.

nodeJS를 사용하기 위해 Jenkins 관리 > Global Tool Configuration 으로 들어간다. 그리고 NodeJS를 찾아서 name에 node를 써준다. 여기서 주의할 점은 nodeJS의 버전을 로컬에 있는 프로젝트와 같은 버전으로 맞춰줘야 한다는 것이다.

![image](https://user-images.githubusercontent.com/50227342/124086943-7f3e5480-da8c-11eb-8a66-f88736adae35.png)

tools에 nodejs 'node(아까 쓴 이름)' 을 써주고, npm install과 npm run build는 sh로 실행해준다.

빌드가 되었다면 workspace에서 dist 폴더 안에 빌드 결과물이 있음을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/50227342/124112355-2e872580-daa5-11eb-8207-68a84dc89892.png)

## docker pipeline 을 이용해서 도커 컨테이너 내부 파일들을 로컬로 이동시키기

프롬프트 창에서 해당 명령어를 이용해 실행되고 있는 도커를 확인한다.
```
docker ps
```
![image](https://user-images.githubusercontent.com/50227342/124129420-bdea0400-dab8-11eb-85b8-a8a9c5c85603.png)

2개 중, 위의 jenkins를 써야하므로 이것의 이름을 사용해준다.

그러나 이 시점에서 로컬과 도커를 다시 연결해주기 위해, 도커를 완전히 밀고 다시 시작해야한다는 것을 깨달았다. 그래서 이 뒷 부분의 이야기는...내일인 금요일부터 다시 시작할 것이다....

## docker jenkins 삭제 후 재 설치

도커에 들어가 jenkins를 삭제하고 이 코드로 다시 도커와 로컬을 연결해주어야 한다.
```
docker run -d -u root -p 9090:8080 --name=jenkins -v c:/Users/[사용자 pc이름]/dist:/var/jenkins_home/dist jenkins/jenkins
```
그 후 과정은 위와 동일하다.









