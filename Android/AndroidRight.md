# Android 권한
## 1. 위험 권한
- 대부분 주요 권한들은 개인 정보가 담겨져 있는 정보에 접근하거나 개인정보를 만들어낼 수 있는 단말의 주요 장치에 접근할 때 부여됨   
매니페스트에 넣어준 권한은 앱을 설치할 때 사용자가 허용하면 한꺼번에 권한이 부여되는데  
마시멜로우(API 23)부터는 중요한 권한들을 분류하여 **설치 시점이 아니라 앱을 실행했을 때** 사용자로부터 권한을 부여받도록 변경

### 일반 권한과 위험 권한
- 일반 권한(Nomal Permission) - 앱을 설치할 때 사용자에게 권한이 부여되어야 함을 알려주고 설치할 것인지 물어봄   
-> 사용자가 부여할 권한들을 보고 수락하면 앱이 설치되고 앱에는 해당 권한들이 부여됨
- 위험 권한(Dangerous Permission) - 앱 실행 시 사용자에게 권한을 부여할 것인지 물어봄  
-> 사용자가 권한을 부여하지 않으면 해당 기능은 동작하지 않음   
-> 앱을 설치했다고 하더라도 권한에 따라 실행할 수 있는 기능에 제약이 생기는 것

### 위험 권한의 세부 권한
#### 1. LOCATION
- ACCESS_FINE_LOCATION : Allows an app to access precise location
- ACCESS_COARSE_LOCATION : Allows an app to access approximate location

#### 2. CAMERA
- CAMERA : Required to be able to access the camera device

#### 3. MICROPHONE
- RECORDE_AUDIO : Allows an application to record audio

#### 4. CONTACTS
- READ_CONTACTS : Allows an application to read the user's contacts data
- WRITE_CONTACTS : Allows an application to wirte the user's contact data
- GET_ACCOUNTS : Allows access to the list of accounts in the Accounts Service

#### 5. PHONE
- READ_PHONE_STATE : Allows read only access to phone state, including the current cellular network information, the state of any ongoing calls, and a list of any PhoneAccounts registered on the device

- CALL_PHONE : Allows an application to initiate a phone call without going through the Dialer user interface for the user to confirm the call

- READ_CALL_LOG : Allows an application to read the user's call log

- WRITE_CALL_LOG : Allows an application to write (but not read) the user's call log data

- ADD_VOICEMAIL : Allows an application to add voicemails into the system

- USE_SIP : Allows an application to use SIP service

- PROCESS_OUTGOING_CALLS : This constant was deprecated in API level 29. Applications should use CallRedirectionService instance of the Intent.ACTION_NEW_OUTGOING_CALL broadcast

#### 6. SMS
- SEND_SMS : Allows an application to send SMS messages

- RECEIVE : Allows an application to receive SMS messages

- READ_SMS : Allows an application to read SMS messages

- RECEIVE_WAP_PUSH : Allows an application to receive WAP push messages

- RECEIVE_MMS : Allows an application to monitor incoming MMS messages

#### 7. CALENDER
- READ_CALENDER : Allows an application to read the user's calender data

- WRITE_CALENDER : Allows an application to write the user's calender data

#### 8. SENSORS
- BODY_SENSOR : Allows an application to access data form sensors that the user uses to measure what is happening inside thier body, such as heart rate

#### 9. STORAGE
- READ_EXTERNAL_STORAGE : Allows an application to read from external storage

- WRITE_EXTERNAL_STORAGE : Allows an application to write from external storage
