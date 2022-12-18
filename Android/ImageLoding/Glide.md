# Glide
- Google에서 개발해서 밀고 있었던 volly이후에 2014년에 공개된 라이브러리
- Bump앱을 Google이 인수하면서 Bump앱에서 사용하던 이미지 라이브러리를 공개한 것이 Glide이다.
- 기존의 Picasso에서 사용하는 함수 방식과 거의 비슷하다.(일부 함수를 빼고는 거의 똑같다.)
- 다른 이미지로딩 라이브러리에는 제공하지 않는 썸네일보기, GIF 로딩, 동영상 스틸 보기 기능까지 지원한다.
## 1. Gradle에 추가
 ~~~
implementation 'com.github.bumptech.glide:glide:4.9.0'
annotationProcessor 'com.github.bumptech.glide:compiler:4.9.0'
 ~~~

 ## 2. 사용법
 ~~~
 Glide.with(this).load("http://www.selphone.co.kr/homepage/img/team/3.jpg").into(imageView);
~~~

## 3. 유용한 함수
- override()   
: 지정한 이미지의 크기만큼만 불러올 수 있다.   
이를 통해 이미지 로딩 속도를 최적화할 수 있다.

- placeholder()   
: 이미지를 로딩하는 동안 처음에 보여줄 placeholder이미지를 지정할 수 있다.

- error()  
: 이미지 로딩에 실패했을 경우 보여줄 이미지를 지정할 수 있다.

- thumbnail()  
: 지정한 %비율 만큼 미리 이미지를 가져와서 보여준다.   
0.1f로 지정했다면 실제 이미지 크기 중 10%만 먼저 가져와서 보여준다.

- asGif()  
: 정적인 이미지 뿐만 아니라 GIF도 로딩할 수 있다.