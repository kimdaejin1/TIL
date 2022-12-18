# Picasso
- UIL이후 최근에 가장 널리 쓰이고 있는 이미지로딩 라이브러리
- 별다른 설정 작업없이 직관적으로 함수 호출 하면된다.
## 1. 사용법
1. 기본 사용
    - layout에 ImageView를 추가 하거나 소스상에서 ImageView를 생성하여 Picasso를 적용한다.
        ~~~Xml
        ImageView picassoImageView = (ImageView) findViewById(R.id.picassoImageView);
        Picasso.with(this)
            .load("https://www.google.co.kr/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")
            .into(picassoImageView);
        ~~~
        위와 같이 별다른 네트워크 소스 없이 ImageView에서 네트워크상의 이미지를 가져온다.

2. 기본 이미지 설정
    - 이미지 로드전 또는 로드에 실패하면 별도의 이미지를 지정하여 ImageView에 나오도록 지정할 수 있다.   
    placeholder를 추가하여 사용한다.
        ~~~Xml
        Picasso.with(this)
        .load("https://dwfox.tistory.com")
        .placeholder(R.drawable.dwfox)
        .into(picassoImageView);
        ~~~

3. 기본 이미지 크기 조절 (Resize)
    - 이미지 로드전 또는 로드에 실패하면 별도의 이미지를 지정하여 ImageView에 나오도록 지정할 수 있다.   
    큰 이미지를 그대로 로드하면 OutOfMemory를 발생시킬 수 있으므로 필요한 사이즈로 Resize하여 사용할 수 있도록 한다.
        ~~~
        Picasso.with(this)
        .load("https://www.google.co.kr/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")
        .placeholder(R.drawable.dwfox)
        .resize(50,50)
        .into(picassoImageView);
        ~~~

4. Transformation을 이용한 크기 조절 (Resize)
    - 기본 Resize시 비율과 상관없이 resize하게 되므로 비율에 따른 이미지를 resize할 수 없는데  
    transformation을 이용하여 resize를 직접 하도록 하면 비율 따라 조절 할 수 있다.

    1. 아래의 Picasso Transformation을 추가한다.
        ~~~Java
        public class PicassoTransformations {

        public static int targetWidth = 200;

        public static Transformation resizeTransformation = new Transformation() {
            @Override
            public Bitmap transform(Bitmap source) {
                double aspectRatio = (double) source.getHeight() / (double) source.getWidth();
                int targetHeight = (int) (targetWidth * aspectRatio);
                Bitmap result = Bitmap.createScaledBitmap(source, targetWidth, targetHeight, false);
                if (result != source) {
                    source.recycle();
                }
                return result;
            }

                @Override
                public String key() {
                    return "resizeTransformation#" + System.currentTimeMillis();
                }
            };
        }
        ~~~
    
    2. 기본 resize 대신 아래와 같이 Transformation을 이용하여 Resize한다.
        ~~~
        PicassoTransformations.targetWidth = 50;

        Picasso.with(this)
                .load("https://www.google.co.kr/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")
                .placeholder(R.drawable.dwfox)
                .transform(PicassoTransformations.resizeTransformation)

                .into(picassoImageView);
        ~~~
5. 이미지 캐싱과 저장 정책 설정
    - 기본 이미지 라이브러리는 이미지 캐싱과 저장의 이점으로 사용하게 되는 특별하게 이미지 캐싱과 저장을 하지 않아야 할 경우 아래와 같은 옵션으로 저장과 캐싱을 방지할 수 있다.
        ~~~
        Picasso.with(this)
            .load("https://www.google.co.kr/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")
            .placeholder(R.drawable.dwfox)
            .memoryPolicy(MemoryPolicy.NO_CACHE, MemoryPolicy.NO_STORE)
            .networkPolicy(NetworkPolicy.NO_CACHE, NetworkPolicy.NO_STORE)
            .resize(50, 50)
            .into(picassoImageView);
        ~~~