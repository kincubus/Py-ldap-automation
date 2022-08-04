# ldap_automation_bat
## 스크립트 정보
AD서버 Google WorkSpace(G suite) 연동
AD서버 ldap 계정 생성 및 관리
개인정보 취급자 대상 패스워드 변경주기(ISMS-P요건) 알림 적용

스케줄러 예약 인스턴스 경로: FC-AD-1@C:\Manager\Py-Ldap-Manager

## 스크립트 설명
### 1. gcp api token 발급
Localhost:80 포트 오픈 후 
API_Token_Create/login.html 실행

Google 로그인 > Auth 승인 > GCP oAuth 클라이언트 ID, 클라이언트 보안비밀 삽입
> response json 값 중 refresh_Token 저장

### 2. 변수 설정
Properties/conf.json
```json
    "client_id": [oAuth client ID],
    "client_secret": [oAuth client Secret],
    "refresh_token": [refresh_token],
```
GCP oAuth 클라이언트 ID
GCP oAuth 클라이언트 보안비밀
login.html에서 받은 refresh_Token 순서대로 삽입

### 3. GWS 유저 연동
```console
> python GWS_Sync.py
```
 - GWS유저 목록 검색 
 - iam db 유저 목록과 대조
 - 신규 유저의 경우 db insert
 - 퇴사자의 발생 시 db update (퇴사여부/퇴사 날짜)
 - 퇴사자 중 30일 경과 시 iam DB에서 삭제

### 4. AD 계정 관리
```console
> python AD_Sync.py
```
 - AD계정과 iam DB유저 현황 비교
 - 신규 유저의 경우 AD 계정 생성
 - 퇴사자 발생시 AD 계정 비활성화 (삭제x)
 - 퇴사자 중 30일 결과 시 AD계정 삭제
 - prv-net 및 fcusers(vpn) 그룹 추가

### 5. 패스워드 변경 주기 알림
```console
> python GMAIL_SEND.py
```
 - 패스워드 변경일 기록(IAM 서비스)을 기준으로 90일 결과 시 계정정보 초기화
 - 초기화 D-10일 부터 대상 계정(메일)에 알림 메일 발송
 - 계정 관리자 (김대석/구민준/유지현)에게 현황 메일 발송


## 설치 및 구동
### install 
```console
> pip install ldap
> pip install pymysql
> pip install requests
```

### start
```console
> ./LDAP Scheduler.bat
```
