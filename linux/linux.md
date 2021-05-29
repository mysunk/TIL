linux struggles
=======
linux commands and studies  

### Symbolic link
* 원본 파일을 가리키도록  링크하는 바로가기 파일
* 원본 파일의 크기와 무관
* 원본 파일이 삭제되면 링크하는게 없어짐 
```commandline
ln -s [원본파일] [대상파일]
```

### vscode 원격 접속
* server에 openssh-server 설치 확인
* local접속시 port번호는 22로 설정

### 원격 접속 configuration setting
1. config 파일 디렉토리
```commandline
C:/Users/user_name/.ssh/config
```
2. 형식
```commandline
Host anything
HostName 172.26.199.3
User name_of_host
Port 22
```
