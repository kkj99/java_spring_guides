# Tomcat 8.5.13 설치 가이드 

## download
1. windows
- 커맨드창에서 실행
   ```shell
   # Tomcat 다운로드
   bitsadmin /transfer "download tomcat binary" ${binary_link} ${path}tomcat.zip 
   ```
- ${binary_link} : 다운로드할 maven 바이너리 주소. [확인](https://maven.apache.org/download.cgi)
    - 8.5.13
      - windows 64bit : https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.13/bin/apache-tomcat-8.5.13-windows-x64.zip
      - windows 32bit :https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.13/bin/apache-tomcat-8.5.13-windows-x86.zip
      - zip : https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.13/bin/apache-tomcat-8.5.13.zip
      - tar.gz : https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.13/bin/apache-tomcat-8.5.13.tar.gz
- ${path} : 다운로드 위치


3. Linux
   ```shell
   wget -O ./tomcat.zip ${binary link}  
   ```

## 압축 해제

2. windows
* 알집이나 반디집과 같은 압축 프로그램으로 압축을 해제한다.
* 명령어 만으로 압축해제
    * 커맨드창에서 다음 명령어를 한단계씩 실행한다 (※ 커맨드창은 중간에 닫고 새로 열지 않는다.)
   ```shell
   # 명령어를 실행하는 현재 위치를 CURRENT_PATH 환경변수에 저장
   >output.tmp cd 
   <output.tmp ( SET /P CURRENT_PATH= ) 
   del /f output.tmp
   ```
   ```shell
   # unzip.exe 파일 다운로드
   bitsadmin /transfer "download unzip.exe" https://github.com/kkj99/java_spring_guides/raw/main/unzip.exe %CURRENT_PATH%\unzip.exe
   ```
   ```shell
   # 현재 위치\mvn 폴더에 압축 해제
   unzip.exe -x %CURRENT_PATH%\tomcat.zip -d %CURRENT_PATH%\tomcat
   ```
   ```shell
   # 필요없어진 파일들 삭제
   del /f unzip.exe & del /f tomcat.zip
   ```
3. Linux
   ```shell
   unzip -x ./tomcat.zip -d ./tomcat
   ```
## 설치 확인
1. windows
* 설정 확인
  ```shell
  ${tomcat 폴더}\configtest.bat
   
  # Using CATALINA_BASE: ~~~
  # Using CATALINA_HOME: ~~~
  # Using CATALINA_TMPDIR: ~~~
  # Using JRE_HOME: ~~~
  # Using CLASSPATH: ~~~
  # ...
  # ...
  # ...
  # 13-Oct-2022 21:46:12.746 정보 [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
  # 13-Oct-2022 21:46:13.057 정보 [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
  # 13-Oct-2022 21:46:13.059 정보 [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["ajp-nio-8009"]
  # 13-Oct-2022 21:46:13.061 정보 [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
  # 13-Oct-2022 21:46:13.062 정보 [main] org.apache.catalina.startup.Catalina.load Initialization processed in 1073 ms
  ```
  
* 버전 확인
     ```shell
      ${tomcat 폴더}\catalina.bat version
       
      # Using CATALINA_BASE:   ~~~
      # Using CATALINA_HOME:   ~~~
      # Using CATALINA_TMPDIR: ~~~
      # Using JRE_HOME:        ~~~
      # Using CLASSPATH:       ~~~
      # Server version: Apache Tomcat/8.5.13
      # Server built:   Mar 27 2017 14:25:04 UTC
      # Server number:  8.5.13.0
      # OS Name:        Windows 10
      # OS Version:     10.0
      # Architecture:   amd64
      # JVM Version:    1.8.0_322-b06
      # JVM Vendor:     Azul Systems, Inc.
      ```
  
2. linux
* 실행 가능하도록 permission 설정
    ```shell
    chmod +x ${tomcat 폴더}/bin/*.sh
    ```
* 설정 확인

    ```shell
    ${tomcat 폴더}/bin/configtest.sh

    # Using CATALINA_BASE:   ~~~
    # Using CATALINA_HOME:   ~~~
    # Using CATALINA_TMPDIR: ~~~
    # Using JRE_HOME:        ~~~
    # Using CLASSPATH:       ~~~
    # ...
    # ...
    # ...
    # 정보: Initializing ProtocolHandler ["http-nio-8080"]
    # 10월 13, 2022 9:51:39 오후 org.apache.tomcat.util.net.NioSelectorPool getSharedSelector
    # 정보: Using a shared selector for servlet write/read
    # 10월 13, 2022 9:51:39 오후 org.apache.coyote.AbstractProtocol init
    # 정보: Initializing ProtocolHandler ["ajp-nio-8009"]
    # 10월 13, 2022 9:51:39 오후 org.apache.tomcat.util.net.NioSelectorPool getSharedSelector
    # 정보: Using a shared selector for servlet write/read
    # 10월 13, 2022 9:51:39 오후 org.apache.catalina.startup.Catalina load
    # 정보: Initialization processed in 311 ms
   ```
  * http 포트번호 8080 확인 ( conf/server.xml 설정파일 에서 포트번호를 수정했다면 다를 수 있다. )
  

* 버전 확인
   ```shell
    ${tomcat 폴더}/bin/catalina.sh version
  
    # Using CATALINA_BASE:   ~~~
    # Using CATALINA_HOME:   ~~~
    # Using CATALINA_TMPDIR: ~~~
    # Using JRE_HOME:        ~~~
    # Using CLASSPATH:       ~~~
    # Server version: Apache Tomcat/8.5.13
    # Server built:   Mar 27 2017 14:25:04 UTC
    # Server number:  8.5.13.0
    # OS Name:        Linux
    # OS Version:     5.15.0-46-generic
    # Architecture:   amd64
    # JVM Version:    1.8.0_342-8u342-b07-0ubuntu1~20.04-b07
    # JVM Vendor:     Private Build
   ```