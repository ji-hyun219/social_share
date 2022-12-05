# social_share

소셜미디어에 사진 공유 연습 레포지토리

## R.java

안드로이드 앱 개발을 시작하면, 화면 레이아웃을 표시하고, View를 참조하기 위해 아래와 같은 코드를 작성한다.

```java
 setContetView(R.layout.activity_main);
 fineViewById(R.id.text_view_01)
```

`R.java` 란?  
안드로이드 리소스(레이아웃, 이미지, 문자열 등)를 식별하기 위한 변수들을 관리하는 `R 클래스`이다.

예를 들어,

1. res/drawable 경로에 my_image_01.png 파일을 추가하면,

2. R.java 파일에 정수 형태의 "my_image_01" 변수가 자동으로 생성되며,

3. 다른 자바 코드에서 "R.drawable.my_image_01" 으로 해당 이미지 리소스를 참조할 수 있게 된다.

```java
/* R.java 클래스 */
public final class R{

  // 이미지 리소스 관리
  public static final drawable{
    // ...
    public static final int my_image_01 = 0x32f2729 // 새로 추가한 이미지
  }

  // 문자열 리소스 관리
  public static final string{
    // ...
    public static final int xxx = 0x52f2723
  }

  // 레이아웃 리소스 관리
  public static final layout{
    // ...
    public static final int xxx = 0x72f2722
  }

}
```

마찬가지로, res/layout 경로의 activity_main.xml 파일은 R.layout.activity_main으로 참조할 수 있다.

## 메니페스트 파일

앱의 패키지, 컴포넌트, 권한, 기기호환성 설정을 관리하는 app > manifest > AndroidManifest.xml 파일이다.

<manifest>: 패키지

ㄴ <application>: 컴포넌트

ㄴ <uses-permission>: 권한

ㄴ <uses-feature>: 기기호환성

1. 패키지

- 설정 내용: 앱의 식별자인 패키지 정보를 등록한다.

- 설정 위치: manifest 태그의 package속성

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.projectstudy_java">
```

- 패키지는 앱을 구별하는 식별자로 작용한다.

- R.java에서 리소스를 찾을 때에도 패키지 정보를 활용한다.

- 패키지 정보를 이용해서, .으로 시작하는 클래스를 "패키지.클래스"로 해석한다. (예: `.MainActivity 는 com.example.social_share.MainActivity 과 동일.. 바꿔써도 상관무`)

2. 앱의 구성요소: 컴포넌트(Components)

- 설정 내용: 앱을 구성하는 `액티비티, 서비스, 컨텐트 프로바이더, 브로드캐스트 리시버`를 등록한다.

- 설정 위치: manifest > appliaction > `activity|service|provider|receiver` 태그

```xml
<manifest>
    <application>

        <activity android:name=".MainActivity">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>
</manifest>
```

- 상위 태그인 application에는 아이콘, 라벨 등의 속성 값을 설정한다.

- 하위 태그인 `intent-filter`에는 `암시적 인텐트(Intent)`를 통한 컴포넌트 실행정보를 등록한다.

### 암시적 인텐트

암시적 인텐트는 작업을 지정하여 기기에서 해당 작업을 수행할 수 있는 모든 앱을 호출할 수 있도록 합니다. 본인의 앱은 작업을 수행할 수 없지만 다른 앱은 그 작업을 수행할 수 있는 가능성이 있고, 사용자가 어떤 앱을 사용할지 선택하기를 원할 경우에 암시적 인텐트가 유용합니다.

- 작업
  수행할 일반적인 작업을 나타내는 문자열입니다.  
  `ACTION_VIEW` : 이 작업은 액티비티가 사용자에게 표시할 수 있는 어떤 정보를 가지고 있을 때 startActivity() 가 있는 인텐트에서 사용합니다. (예: 갤러리 앱에서 볼 사진이나, 지도 앱에서 볼 주소)

`ACTION_SEND` : 공유 인텐트라고도 하며, 사용자가 다른 앱을 통해 공유할 수 있는 데이터를 가지고 있을 때 startActivity() 가 있는 인텐트에서 사용해야 합니다. 예를 들어 이메일 앱 또는 소셜 공유 앱 등이 이에 해당됩니다.

- 카테고리
  인텐트를 처리해야 하는 구성 요소의 종류에 관한 추가 정보를 담은 문자열입니다.  
  인텐트 안에는 카테고리 설명이 얼마든지 들어 있을 수 있지만, 대부분의 인텐트에는 카테고리가 없어도 됩니다.  
  `CATEGORY_BROWSABLE` : 대상 액티비티가 웹 브라우저를 통해 시작되도록 허용하고 이미지, 이메일 메시지 등의 링크로 참조된 데이터를 표시하게 합니다.  
  `CATEGORY_LAUNCHER` : 이 액티비티가 작업의 최초 액티비티이며, 시스템의 애플리케이션 시작 관리자에 목록으로 게재됩니다.

즉 인텐트는 어떤 메시지를 담고 있는 하나의 객체다. 이 메시지는 어떤 액티비티로 이동할지, 어떤 서비스를 시작할지 등 목적지 정보를 갖고 있거나 옮겨야 하는 데이터를 갖고 있을 수도 있다.

명시적 인텐트는 아예 대상 이름을 콕 찝어서 그 대상에게 어떤 작업을 하라고 명령할 때 사용하는 것이다. 때문에 자바코드에서 액티비티, 프래그먼트, 서비스 등 컴포넌트를 실행할 때 사용한다. 암시적 인텐트는 대상 이름을 콕 찝어 말하진 않지만, 비슷한 기능을 수행하는 앱들을 불러모아서 사용자한테 이 중에 선택해서 쓰라고 할때 사용하는 인텐트이다. 대표적인 예시가 PDF 를 열거나 파일 공유 시, 음악/영상 재생 시, 카메라 앱을 실행시키는 버튼을 눌렀을 때 하단에서 올라오는 앱들의 목록을 담은 Bottom Sheet 다. 설치된 관련 앱들이 있어도 사용자의 핸드폰에 그 앱이 깔려있지 않을 수 있으니, 일단 같은 기능 하는 앱들을 전부 불러다가 모아서 보여주는 것이다. 사용자는 그 중에서 원하는 앱을 실행하면 된다.  
Bottom Sheet 가 암시적 인텐트가 아니라, Bottom Sheet에 유저가 원하는 기능을 수행하는 앱들을 담아서 보여주도록 하는 요소가 암시적 인텐트란는 것이다.  
또한 암시적 인텐트를 써서 사용자가 앱을 선택하게 하는 순간에 엉뚱한 앱이 나오지 않게 처리해야 할 것이다.  
이때 인텐트를 가려서 앱들을 실행시키기 위해 인텐트 필터를 사용한다.

정리,  
명시적 인텐트는 다음에 수행할 컴포넌트의 이름을 명확하게 제시한다.  
보통은 App 내에 있는 component를 실행할 때 사용한다.  
암시적 인텐트는 구체적인 component의 이름을 제시하지 않는다.  
대신에, 이 인텐트에 어떤 동작을 수행할지 선언한다.  
암시적 인텐트를 사용하면, 안드로이드 시스템은 다른 app들에 있는 manifest의 intent filter 를 참조해서 인텐트에 담긴 작업을 수행할 수 있는 component 들을 찾아 내서 사용자에게 어떤 app을 사용해서 작업을 수행할지 선택지를 준다.

이것을 통해서 이 동작을 수행할 수 있는 다른 app에게 작업을 수행할 것을 요청한다.

3. 권한

- 설정 내용: 전화걸기, 연락처접근 등의 각종 권한정보를 등록한다.

- 설정 위치: manifest > uses-permissions 태그

```xml
<manifest>

    <uses-permission android:name="android.permission.SEND_SMS"/>

</manifest>
```

4. 기기 호환성

- 설정 내용: 앱에서 요구하는 하드웨어/소프트웨어 기능 및 호환되는 기기유형을 등록한다.

- 설정 위치: manifest > uses-feature 태그

```xml
<manifest>

    <uses-feature android:name="android.hardware.sensor.compass"
                  android:required="true" />

</manifest>
```

- 위에서는 필요한 사양으로 나침반 센서 등록했다.

- 따라서 나침반 센서가 탑재되지 않은 스마트폰은 Google Play Store 에서 설치가 불가능할 것이다.
