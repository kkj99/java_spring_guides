# java_spring_4_3_2
- Spring framework로 개발된 sample Web Application 입니다.
- 본 가이드는 java spring 기반 smaple web application 에 대한 공통 가이드 입니다.
-----
## Application 기본 구성사항
- 각 sample application repository 참고
- [4.3.2](https://github.com/kkj99/java_spring_4_3_2)
- [4.3.4](https://github.com/kkj99/java_spring_4_3_4)
- [4.3.13](https://github.com/kkj99/java_spring_4_3_13)
- [5.2.7](https://github.com/kkj99/java_spring_5_2_7)
- 접속권한이 없으면 접근할 수 없습니다. 필요시 [문의](mailto:kkj99@nets.co.kr) 하세요 

## JDK 1.8, Tomcat 8.5.13 환경에서 동작이 확인되었습니다
- Zulu OpenJDK 1.8
- Tomcat 8.5.13 Tested

## 빌드 및 실행
### 준비사항
- JDK 1.8+ : [DOWNLOAD 링크](https://www.azul.com/downloads/?package=jdk#download-openjdk)
- Tomcat 8.X.X : [DOWNLOAD 링크](https://tomcat.apache.org/download-80.cgi)
- MAVEN : [DOWNLOAD 링크](https://maven.apache.org/download.cgi) /  [INSTALL 가이드](https://maven.apache.org/install.html)

### 빌드
1. Git Clone
- 필요한 sample spring java application repository 로부터 git clone
- [Github Generate new SSH Key 가이드](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [Github Creating peronsla access token 가이드](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
```shell
git clone https://github.com/{repository}.git 
```


2. Maven Build
- 소스 루트에서 명령어 실행
```shell 
mvn package
```
- 빌드가 완료되면 target/spring-sample-1.0-SNAPSHOT.war 파일이 생성됩니다.

3. Deploy tomcat
- Maven Build로 생성된 war 파일을 이용해 배포합니다.
- $CATALINA_HOME/webapps/{warfilename}.war 과 같이 war파일을 복사하고 tomcat을 startup 합니다.
- 메인페이지 접속을 확인합니다 : http://localhost:{server port}/{warfilename}/index.do

## 주요 page
- index.do : 메인 페이지
- login.do : 로그인 페이지
- normal.do : 인증이 불필요한 일반 페이지
- secure.do : 인증이 필요한 보안 페이지
- logout.do : 로그아웃 페이지

-----

## Challenge

1. 메인 페이지
    1. OKTA에 인증된 상태라면, 로그인된 사용자 정보와 로그아웃 링크 표시
    2. OKTA에 인증딘 상태가 아니라면, 로그인 링크를 표시하고 로그인 링크 클릭시 로그인 페이지로 이동

2. 로그인 페이지
    1. 이미 OKTA에 인증되어 있다면, 인증 된 사용자 정보를 Session에 저장하고 메인 페이지로 이동
    2. OKTA에 인증된 상태가 아니라면, OKTA 로그인 페이지 표시
    3. 표시된 OKTA 로그인 페이지에서 로그인 성공후 메인 페이지로 이동

3. 인증이 필요한 보안 페이지
    1. OKTA에 인증된 상태라면, 로그인된 사용자 정보 표시
    2. OKTA에 인증딘 상태가 아니라면, 로그인 페이지로 이동
    3. 이동된 로그인 페이지에서 OKTA 로그인 성공후 원래 페이지로 돌아올것

4. OKTA 테넌트에 Application으로 등록하고 사용자에게 할당한다
    1. 할당한 사용자로 로그인하면 Dashboard에 AppCard가 표시되고 클릭을 통해 Application 메인 페이지로 인증된 상태로 이동한다
-----

## Test 시나리오
### OKTA 인증되지 않은 상태
1. 메인페이지 접속
    1. 로그인 링크가 표시된다
    2. 로그인 링크로 로그인 페이지 이동시 OKTA 로그인 페이지가 표시된다
    3. 표시된 OKTA 로그인 페이지에서 인증 성공하면 메인 페이지로 이동된다
    4. 인증후 이동된 메인 페이지에서는
        1. 로그인 링크가 더이상 표시되지 않는다
        2. 로그인된 사용자 정보가 표시된다
        3. 로그아웃 링크가 표시된다
2. 로그인페이지 접속(직접 주소 입력)
    1. OKTA 로그인 페이지가 표시된다
        3. OKTA 인증 성공하면 메인 페이지로 이동된다
        4. 인증후 이동된 메인 페이지에서는
            1. 로그인 링크가 더이상 표시되지 않는다
            2. 로그인된 사용자 정보가 표시된다
            3. 로그아웃 링크가 표시된다
3. 보안페이지 접속(직접 주소 입력)
    1. 로그인 페이지로 자동 이동되어 OKTA 로그인 페이지가 표시된다
        1. OKTA 인증 성공하면 보안페이지로 이동된다
        2. 인증후 이동된 보안페이지에서는
            2. 로그인된 사용자 정보가 표시된다
            3. 로그아웃 링크가 표시된다

### OKTA 인증된 상태
1. 메인페이지 접속
    1. 로그인된 사용자 정보가 표시된다
    2. 로그아웃 링크가 표시된다.
2. 로그인페이지 접속 (직접 주소 입력)
    1. 메인페이지로 이동된다
3. 보안페이지 접속
    1. 로그인된 사용자 정보가 표시된다
    2. 로그아웃 링크가 표시된다
4. 로그아웃 링크 클릭 ( 또는 직접 주소 입력 )
    1. OKTA 로그아웃 후 메인페이지로 이동된다
