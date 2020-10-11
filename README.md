## AMDM(Army Mobile Device Management)
저희는 RFID 기술을 이용한 IoT 장치와 휴대전화를 통제하는 APP을 개발하여 군에서의 보안/안전적인 측면을 더욱 강화시키는 것 뿐만아니라 전군 장병들의 휴대폰 관리를 전산화 처리를 할려고 합니다.
저희 팀은 국방 오픈소스 아카데미에 참가하여 국군 모바일 단말 장치 관리 시스템(Army Mobile Device Management System)을 구현시키고자 합니다. 🔥

## 팀소개 및 프로잭트 설명 동영상 👍
[![Watch the video](https://img.youtube.com/vi/LjX3eVQdIyk/0.jpg)](https://www.youtube.com/watch?time_continue=117&v=LjX3eVQdIyk)

## 기능 설계
 - [안드로이드 디자인](https://ovenapp.io/project/qREXQacVLW1qzBvL5xhXo0wNgUoqVt7W#ScIyR)

## 구성 / 필수 조건 안내 (Prerequisites)
* Android 6.0 (Marshmallow) 이상 
* Python 서버가 실행되어있어야 정상적으로 작동함

## 기술 스택 (Technique Used)
### Server(back-end)
 - Python 3버전을 통해 개발
 - TCP 소켓 통신 서버
 - SQLite로 DB 구현

### IoT

### Android
 - *Fragment*를 통하여 메인화면 구현
 - *BroadCast Receiver*를 통해 안드로이드 켜질시 상태 구현
 - chart 구현을 위해 [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)를 사용하여 구현

## 설치 안내 (Installation Process) - 미완성

### Android apk 실행
* ${Git Home}/app-debug.apk

### Python 서버 실행
```bash
$ python3 ${Git Home}/AMDMServer/server.py
```

### SQLite 실행(필요시)
```bash
$ sqlite3
$ .open AMDMserver.sqlite3
```

## 프로젝트 사용법 (Getting Started) - 미완성
1. [Android](https://github.com/yeonghokim/AMDM#android-1)
2. [Server](https://github.com/yeonghokim/AMDM#server)
3. [IoT장비와 데이터 교환](https://github.com/yeonghokim/AMDM#iot%EC%9E%A5%EB%B9%84-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B5%90%ED%99%98)
4. [Android와 데이터 교환](https://github.com/yeonghokim/AMDM#android-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B5%90%ED%99%98)
5. [DataBase(SQLite)](https://github.com/yeonghokim/AMDM#database-sqlite)

## Android
#### 화면 리스트
* 스플래시 activity
* 로그인 메인 activity
* 로그인 activity
* 회원가입 activity
* 메인 activity
    * 잠금 Fragment
    * 통계 Fragment
    * 설정 Fragment
    * TurnOn Activity

## Server
* Python3으로 이루어진 TCP 소켓 서버입니다.

## IoT장비 데이터 교환
### 데이터 수신 형태 `IoT -> Server`
  * **Type** : 기기의 형태
  * **ID** : 기기의 아이디(초기 설정때 서버에서 지급)
  * **Lock** : 현재 기기의 잠금 유무
  * **PhoneLock** : 기기안의 핸드폰의 잠금 상태
```json
{
        "Type": "IoT",
        "ID": 1234567,
        "Lock": 0,
        "PhoneLock": {
                "19-760730001": 0,
                "19-760730002": 0,
                "19-760730003": 1,
                "19-760730004": 0
        }
}
```
### 데이터 송신 형태 `Server -> IoT` 
  * **ForceLock** : 현재 기기의 강제 잠금 유무
  * **PhoneLock** : 기기안의 핸드폰의 강제 잠금 유무
  * **PhoneUnLock** : 기기안의 핸드폰의 강제 잠금 유무
```json
{
        "ForceLock": 0,
        "PhoneLock": {
                "19-760730003": 1
        },
        "PhoneUnLock": {
                "19-760730004": 0
        }
}
```

## Android 데이터 교환
### 데이터 수신 형태 Type1 `Android -> Server` 
  * **Type** : 기기의 형태
  * **RequestType**: 요청 타입
  * **ID** : 기기의 아이디(초기 설정때 서버에서 지급)
  * **Lock** : 현재 기기의 잠금 유무
  * **Time** : 기기의 잠금 시간
```json
{
        "Type": "Android",
        "RequestType": 1,
        "ID": 1234567,
        "Lock": 1,
        "Time": "2020-10-04 13:49:12"
}
```
### 데이터 수신 형태 Type2 `Android -> Server`
  * **Type** : 기기의 형태
  * **RequestType**: 요청 타입
  * **ID** : 기기의 아이디(초기 설정때 서버에서 지급)
  * **IoTID** : IoT기기의 아이디
  * **Lock** : 잠글지 열지
  * **Time** : 요청 시간
```json
{
        "Type": "Android",
        "RequestType": 2,
        "ID": 1234567,
        "IoTID" : 123,
        "Lock" : 1,
        "Time": "2020-10-04 13:49:12"
}
```
### 데이터 수신 형태 Type3 `Android -> Server`
  * **Type** : 기기의 형태
  * **RequestType**: 요청 타입
  * **ID** : 기기의 아이디(초기 설정때 서버에서 지급)
  * **TurnOnTime** : 기기가 켜진 시간
```json
{
        "Type": "Android",
        "RequestType": 3,
        "ID": 1234567,
        "TurnOnTime": "2020-10-04 13:49:12"
}
```

## DataBase (SQLite)

### User Table
칼럼이름 | 타입 | 널 유무  | 외래키 유무
-------- | -------- | ---------- | ----------
USER_PR | INTEGER | PRIMARY | 
USER_ARMYNUMBER | CHAR | X
USER_NAME | CHAR | X
PASSWORD | CHAR | X
USER_DISCHARGEDATE | DATETIME | O 
UPDATEDATE | DATETIME | X
### Phone TableW
칼럼이름 | 타입 | 널 유무  | 외래키 유무
-------- | -------- | ---------- | ----------
PHONE_PR | INTEGER | PRIMARY
USER_UNIQUENUM | INTEGER | X | USER.USER_PR
PHONE_IP | CHAR | X
IS_LOCK | INTEGER | X
### PhoneCase Table
칼럼이름 | 타입 | 널 유무  | 외래키 유무
-------- | -------- | ---------- | ----------
PHONECASE_PR | INTEGER | PRIMARY
IS_LOCK | INTEGER | X
PHONE1_ID | INTEGER | O | PHONE.PHONE_PR
PHONE2_ID | INTEGER | O | PHONE.PHONE_PR
PHONE3_ID | INTEGER | O | PHONE.PHONE_PR
PHONE4_ID | INTEGER | O | PHONE.PHONE_PR
### LockManage Table
칼럼이름 | 타입 | 널 유무  | 외래키 유무
-------- | -------- | ---------- | ----------
LOCKMANAGE_PR | INTEGER | PRIMARY
PHONE_UNIQUENUM | INTEGER | X | PHONE.PHONE_PR
MANAGETIME | DATETIME | X
IS_LOCK | INTEGER | X

## 팀 정보 (Team Information) 💙
- 김영호 (yeongho.kim2000@gmail.com), Github Id: yeonghokim
- 부규필 (@gmail.com), Github Id: Qfeel-Dev

## 저작권 및 사용권 정보 (Copyleft / End User License) - 미완성
 * [linechart](https://github.com/PhilJay/MPAndroidChart)
