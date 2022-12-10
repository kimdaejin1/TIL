# 4대 컴포넌트
## 컴포넌트 (Component) 란?
- 컴포넌트는 __구성요소__ 라는 뜻을 지니고 있다. 다시 말해 안드로이드 4대 컴포넌트는 안드로이드 앱을 구성하는데 필요한 4개의 요소를 이야기 한다.  
4대 컴포넌트에는 **Activity**, **Service**, **BordCast Receiver**, **Content Provider** 가 있다.  
각 컴포넌트는 독립적인 형태로 존재하며, 고유한 기능을 수행하고 **Intent**를 통해 상호작용 한다.
<img src="https://images.velog.io/images/jojo_devstory/post/9138556b-4a4c-4c48-a6dc-c9abc34e9b46/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-03-06%20%EC%98%A4%EC%A0%84%2011.51.43.png">

## 1. Activity
- Activity는 사용자가 Application과 상호작용하며 실제로 사용자에게 보이는 화면을 의미한다.
- Application에 화면이 하나도 없으면, 사용자와 상호작용 할 수 없으므로 적어도 하나의 Activity는 반드시 필요하다.
- 다른 Application의 Activity 역시 Intent를 통해 불러올 수 있다.
- 하나의 **View**또는 **ViewGroup**을 가지고 있어야 한다. 여기서 View는 화면에서 보이는 것들, 예를 들어 텍스트, 버튼 등등을 의미한다.  
ViewGroup은 레이아웃을 의미한다고 볼 수 있다.

## 2. Service
- Service는 Activity와 반대로 직접적으로 상호작용하지 않는 요소이다.
- Background에서 어떠한 작업을 처리하기 위해 사용된다.
- Application이 종료되어도 Background에서 동작하는 컴포넌트이다.

## 3. BordCast Receiver
- 안드로이드 OS로부터 발생하는 이벤트 정보를 받고 대응하는 컴포넌트이다.
- 대부분 Ui를 가지지 않으며, 수신기를 통해 디바이스 상황을 감시하다가 이벤트가 발생하면 해당 이벤트에 맞게 정의한 작업들을 수행하는 역할을 한다. 다시 말해, 디바이스에서 발생하는 중요한 이벤트를 Application에 알려준다.
- 대표적인 예로 배터리 부족, 문자 수신, 전화 수신 과 같은 정보를 받아서 이를 처리할 때 동작한다.

## 4. Content Provider
- Content Provider는 데이터를 관리하고 다른 Application의 데이터를 제공하는 데 사용하는 컴포넌트이다.
- 데이터를 저장하고, 불러와서, 사용할 수 있는 시스템들을 이야기한다.
- 파일 시스템이나 SQLiteDB, 기타 저장소 위치에서 앱이 접근 가능한 저장소의 데이터를 읽거나 쓸 수 있다.
- 용량이 큰 데이터를 공유하는데 적합하다.
