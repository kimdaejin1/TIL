# Activity
## Activity의 생명 주기란?   
* * * 
<img src="https://blog.kakaocdn.net/dn/cs6PT5/btqIDD8T93n/TxDj98W7xC0dq2H2qviLb1/img.png">  
안드로이드는 앱이 실행된 후 다른 Activity로 전환하거나, 스마트폰 화면이 꺼지거나, 앱이 종료될 경우같은 상태변화가 있을 때마다 화면에 보이는 Activity의 생명주기 메서드를 호출해 상태 변화를 알려준다.  

## 생명 주기 메서드 
* * *   
<img src="https://blog.kakaocdn.net/dn/bhpxJ2/btqIs9BGPbQ/KcbZ48j6rWG6kMrLuQq9Zk/img.png">    

## 생명 주기 호출 
* * *   
- Activity는 인스턴스 생성과 동시에 생성관련 생명 주기 메서드를 순차적으로 호출 된다.   
종료시 소멸과 관련된 생명 주기 메서드가 순차적으로 호출 된다.

   ### Activity 생성   
   1. onCreate() -> 생성된 화면 구성요소를 메모리에 로드
   2. onStart(), onResume() -> 화면의 구성요소를 나타내고 사용자와 상호작용 시작(Resumed 실행 중)
<img src="https://blog.kakaocdn.net/dn/JyQBC/btqIyeoVto4/rkQMn597pKDfxLhkvTddi0/img.png">

   ### Activity 화면 제거
   1. onPause(), onStop() -> 뒤로 가기, finish()를 실행할 때 동시에 실행
   2. onDestroy() -> 최종적으로 Activity를 메모리에서 제거
<img src="https://blog.kakaocdn.net/dn/OILQN/btqIs99uPDN/Vlc7V3VJMmqXdaFtg4yxR1/img.png">

   ### Activity를 종료하지 않고 다른 Activity 실행
   1. onPause(), onStop() -> 현재 Activity를 종료하지 않고 새로운 Activity가 만들어 질때 (Stopped)
   2. onStart(), onResume() -> 두 메서드가 연속적으로 실행되고 Resumed 상태로 변경
<img src="https://blog.kakaocdn.net/dn/ceHKGd/btqIAOpvDcn/S2Hc6Fwe1ev4m3k3nPJ7Mk/img.png">
    
   ### Activity를 종료하지 않거나, 모두 가려지지 않을 때 Activity 실행
   1. onPause() -> 완전히 사라진 것은 아니기 때문에 Paused로 변경
   2. onResume() -> 정지가 아니니까 onStart를 거치지 않고 바로 onResume로 Resumed
<img src="https://blog.kakaocdn.net/dn/bfuNQN/btqIvEBoMxT/HMn9pxgjoPTDbOocbmorB1/img.png">
