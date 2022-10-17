# MAVEN 설치 가이드

배포 바이너리를 다운로드 받아 압축을 해제합니다.

-----

## download 
   1. windows 
   - 커맨드창에서 실행
      ```shell
      bitsadmin /transfer "download maven binary" ${binary link} ${path}maven.zip 
      ```
   - binary link : 다운로드할 maven 바이너리 주소. [확인](https://maven.apache.org/download.cgi)
     - 2022-10-13 일자 최신 버전 : 3.8.6 → https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.zip
   - path : 다운로드할 경로


   3. Linux
      ```shell
      wget -O ./maven.zip ${binary link}  
      ```

-----

## 압축 해제

2. windows 
* 알집이나 반디집과 같은 압축 프로그램으로 압축을 해제한다.
* 명령어 만으로 압축해제
  * 커맨드창에서 다음 명령어를 한단계씩 실행한다 (※ 커맨드창은 중간에 닫고 새로 열지 않는다.)
    * 명령어를 실행하는 현재 위치를 CURRENT_PATH 환경변수에 저장
   ```shell
   >output.tmp cd 
   <output.tmp ( SET /P CURRENT_PATH= ) 
   del /f output.tmp
   ```
  
  * unzip.exe 파일 다운로드
   ```shell
   bitsadmin /transfer "download unzip.exe" https://github.com/kkj99/java_spring_guides/raw/main/unzip.exe %CURRENT_PATH%\unzip.exe
   ```

  * 현재 위치\mvn 폴더에 압축 해제
   ```shell
   unzip.exe -x %CURRENT_PATH%\maven.zip -d %CURRENT_PATH%\mvn
   ```

   * 불필요한 파일 삭제
   ```shell
   del /f unzip.exe & del /f maven.zip
   ```
3. Linux
   ```shell
   unzip -x ./maven.zip -d ./mvn
   ```

## PATH 등록 (선택사항)
* 어느경로에서든 maven을 실핼 할 수 있게 하기 위해서는 PATH 환경변수에 압축해제한 maven의 bin 폴더 경로를 추가해야 한다.
1. Windows
    ```shell
    setx path "%PATH%;${MAVEN bin 폴더 경로}"
    ```

2. Linux
   ```shell
   echo "export PATH=$PATH:${MAVEN bin 폴더 경로}" >> ~/.bashrc
   source ~/.bashrc
   ```

## 설치 확인

* 버전 정보를 출력하게 하여 정상적으로 실행되는지 확인한다.
    ```shell
    mvn -v
    ```
  
  > 출력내용 예시 
  > ```text
  > Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
  > Maven home: ~~~
  > Java version: 1.8.0_342, vendor: Private Build, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
  > Default locale: ko_KR, platform encoding: UTF-8
  > OS name: "linux", version: "5.15.0-46-generic", arch: "amd64", family: "unix"
  > ```
