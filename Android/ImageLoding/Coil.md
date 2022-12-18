# Coil
- Coroutine Image Loader의 약자이며 Kotlin Coroutines(코루틴)으로 만들어진 가벼운 Android 백앤드 이미지 로딩 라이브러리

## 장점
1. Glide, Picasso보다 상대적으로 가볍다.
2. 코루틴이 기본이지만 메인까지는 아니며 심플함과 최소한의 보일러 플레이트(boilerplate) 를 위하여 Kotlin의 기능을 활용하여 Kotlin을 잘 다룬다면 사용하기 쉽다.
3. 메모리와 디스크의 캐싱, 메모리의 이미지 다운 샘플링, Bitmap 재사용, 일시정지/ 취소의 자동화 등등 수많은 최적화 작업을 수행하므로 처리속도가 굉장히 빠르다.
4. 다이나믹 이미지 샘플링을 지원하며 이는 Coil에서 자동으로 알아서 처리된다.
5. Coil의 이미지 파이프라인(PipeLine)은 크게 세가지로 구성된다.   
Mappers, Fetchers, Decoders 이러한 인터페이스를 사용하여 기본이미지 로딩 기능을 강화하거나 재정의 하고 Coil에 새로운 파일 형식에 대한 지원을 추가할 수 있다.
6. Kotlin으로 개발되었으며 Coroutines, OkHttp, Okio, AndroidX Lifecycles 등 최신 라이브러리를 사용한다.

## 단점
1. 최소 Android SDK 버전 14부터 지원을 하나 최신버전에서 사용하는 것을 권장함.(AndroidX)
2. Kotlin을 사용할 줄 모르면 어렵다.
3. 일부 기능 사용을 위해 Coroutines이 무엇인지 개념적으로 알아야 한다.

## 초기 적용법
1. gradle에 Coil을 추가한다.
    ~~~Kotlin
    implementation("io.coil-kt:coil:0.10.0")
    ~~~
