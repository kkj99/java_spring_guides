# Okta Org2Org Integration

## Reference

[Integrate Okta Org2Org with Okta](https://help.okta.com/en-us/Content/Topics/Provisioning/org2org/org2org-integrate.htm)

## Goal

* Okta 테넌트간의 인증 연계   
  * A Okta 테넌트에서 B Okta 테넌트의 인증 통합   
  * A Okta 의 사용자 대쉬보드에서 App Link로 B Okta 대쉬보드 접근시 인증 통합
  * B Okta에 통합된 App을 A Okta 대쉬보드를 통해서 접근
   
## Definitions

### Central Org : 중앙 테넌트 
### Target Org : 중앙 테넌트에 인증 통합할 대상 테넌트


# Target Org 에서 Org2Org Application 구성

1. Target Org의 관리 콘솔에서
   1. Applications > Applictaions 메뉴로 이동
   2. [Click] Brows App Catalog
   3. 검색창에 "org2org" 입력 후 Okta Org2Org 선택
   4. [Click] Add Integration
   5. General Settings 단계
      1. 에서 정보를 입력하고 [Click] Next
   6. Sign-on Options 단계
      1. [Select] SAML 2.0
   7. 다른 설정은 아직 설정/입력 하지 않고 그상태로 [Click] Done

# Org2Org Application SAML 설정

1. Target Org의 관리 콘솔에서
   1. Applications > Applictaions 메뉴로 이동
   2. 앞에서 추가한 Org2Org application 선택
   3. Sign-on 탭 에서
      1. 우측 하단 [Click] View SAML setup instructions
   4. 팝업되는 화면의 지침을 따라 설정 진행
      1. [주요내용] Central Org에 IDP 구성. 이 때 Target Org에서 생성하는 인증서 사용
      2. [주요내용] Central Org의 IDP 구성정보로 Target Org의 Org2Org application의 sign-on 설정

# Central Org의 applicaion을 Target Org와 공유

> 예상 시나리오   
> 
> Target Org.dashboard 에서 App 접근   
> 해당 App은 Central Org에 등록된 App   
> 해당 App 접근시 Target Org - Central Org 간의 인증 연계를 통해 인증 처리   
> 
> App에 직접 접근 ( Target Org.dsahboard를 통하지 않은 직접 접근 )   
> App 접근시 인증을 Central Org에 확인   
> Central Org는 인증정보를 Target Org와 연계하여 확인


## Try

1. Central Org에서 공유할 application 준비
2. Target Org 관리 콘솔에서
   1. Applications > Applications 메뉴 이동
   2. [Click] Browse App Catalog
   3. "bookmark" 검색하여 Bookmark App 선택
   4. [Click] Add Integration
   5. URL 값 설정 
      1. {IDP SSO URL}?relayState={App Embedded Link}
      2. IDP SSO URL : Target Org의 org2org application에 설정되는 값
      3. App Embedded Link : Central Org의 공유할 앱 정보의 General 탭에서 확인
         1. OIDC 방식으로 인증되는 application의 경우 implicit flow를 체크해야 App Embedded Link가 하단에 나타남
3. 확인
   1. Target Org에서 추가한 Bookmark App을 사용자에게 할당하고
   2. 할당한 사용자로 로그인하여 Dashboard 통해서 접근
      1. org2org app을 통한 Central Org의 Dashboard 접근은 인증 및 접근이 문제없이 이루어짐
      2. Bookmark App의 경우
         1. Bookmark App은 Central Org와 인증 통합이 되어있고,
         2. Central Org에 로그인 되어 있지 않은 상태에서는 Central Org의 인증창이 나타남
      3. Bookmark App의 General탭 URL값을 {Central Org.idp의 url}?relayState={App Embedded Link} 와 같이 설정한 경우
         1. "요청한 작업을 수행할 수 있는 권한이 없습니다." 메세지와 함께 접근 오류 발생
         2. Central Org의 로그를 보면 Deny 처리된 로그가 확인되며 상세내용은 다음과 같음
            - Event.DisplayMessage : Evaluation of sign-on policy
            - Event.EventType : policy.evaluate_sign_on
            - Event.Outcome.Reason : Sign-on policy evaluation resulted in DENIED
            - Event.Outcome.Result : DENY


### 문제해결


## Reference

[Okta Help Center 질의글](https://support.okta.com/help/s/question/0D54z000077etJCCAY/using-okta-with-another-oktaexternal-identity-provider?language=en_US)

* 질의내용
   * okta와 연동된 App A와, 다른 idp(auth0)와 인증통합된 App B가 있을 때,
   * App B에 접근 시 okta에 인증하고 App B로 이동하게 하는 방법은?
* 답변내용
   * External Identity Provider를 구성해볼것
   * [use the Org2Org app to create a SAML Idp](https://saml-doc.okta.com/SAML_Docs/Configure-SAML-2.0-for-Org2Org.html)
   * configure an OIDC Idp to point to that org (링크 )
   * idp 가 설정되면 Routing Rules 를 생성하여 인증처리할 idp가 지정되게 할 수 있다.


## Target Org의 Bookamrk App 설정

1. Taraget Org의 Bookmark App의 General 탭 URL 값 설정
   1. {IDP SSO URL}?relayState={App Embedded Link}
   2. IDP SSO URL : Target Org의 org2org application에 설정되는 값
   3. App Embedded Link : Central Org의 공유할 앱 정보의 General 탭에서 확인

## Central Org의 공유할 app 접근시 Target Org를 IDP로 인증하도록 설정 

2. Central Org 관리 콘솔에서
   1. Security > Identity Providers 메뉴
   2. Routing Rules 탭
   3. [Click] Add Routing Rule
   4. Rule Context 설정
      - IP
      - device
      - User is accessing : 공유할 app 선택
      - Use this identity provider : Target Org IDP 선택

* 그외 Target Org를 IDP로 인증하길 원하는 Context가 있다면 그에 맞게 설정   
