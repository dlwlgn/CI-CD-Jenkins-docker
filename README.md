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



