# ldap_automation
## 스크립트 정보
AD서버 ldap 계정 생성 및 관리
자주 사용하는 기능을 함수로 엮음

## 스크립트 설명
### 1. 변수 설정
Properties/conf.json
```json
    "LDAPConn": {
    "ldap_address":"ldap://[AD Addr]", 
    "user_name" : [AD ID], 
    "password" : [AD PASSWD]
    }
```
ldap_address : AD 서버 주소
user_name : AD 관리 계정 (ldap 쿼리 권한 필수)
password : 관리 계정의 패스워드


### 2. AD 계정 관리
```console
> python AD_Sync.py
```
 - 신규 유저  AD 계정 생성
 - 삭제 대기 유저 AD 계정 비활성화 (삭제x)
 - AD계정 삭제
 - 그룹 추가



## 설치 및 구동
### install 
```console
> pip install ldap
```

### start
```console
> python AD_Sync.py
```
