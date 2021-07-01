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



















