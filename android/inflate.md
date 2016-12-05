#[Android] Inflate


## Inflate 란?
`Inflate`는 사전적 의미로 **부풀리다**라는 뜻이다. 안드로이드에서 inflate를 사용하면 xml에 씌여져 있는 view 객체로 만드는 역할을 한다. xml을 정의하고 inflate를 하면 view가 생성이 되는 것이다. 

## Infate 의 사용
inflate를 사용하기 위해서는 `inflater`를 얻어와야 한다.
```
LayoutInflater inflater = (LayourInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
```

다음은 `xml`이 필요하다. 이 때 xml의 root view의 type이 무엇인지 알아야 한다. 예를 들어 xml파일 이름이 inflate_example.xml이고, root가 `LinearLayout`이면
```
LinearLayout linearLayout = (LinearLayout) inflater.inflate(R.layout.inflate_example, null);
```

이제 가져온 view를 화면에 그리면 된다.
```
setContentView(learLayout);
```

위의 세 과정을 합치면 다음과 같다. 
```
LayoutInflater inflater = (LayoutInflater) getSystemService( Context.LAYOUT_INFLATER_SERVICE ); 
LinearLayout linearLayout = (LinearLayout) inflater.inflate( R.layout.inflate_example, null ); 
setContentView( linearLayout ); 
```

## Inflate 사용의 또 다른 방법
Inflater를 얻어오는 다른 방법이 있다. 
```
LayoutInflater inflater = LayoutInflater.from(this);
LayoutInflater inflater = getLayoutInflater(); (@activity)
```

인플레이터를 얻어오는 것과 inflate를 하는 것을 한줄로 정리하면 아래와 같다.
```
LinearLayout linearLayout = (LinearLayout) View.inflate(this, R.layout.inflate_example, null);
```

## 추가 정보
Infater는 성능상의 문제 때문에 compile time에 완성된 xml파일에 대해서만 적용 가능하다. 즉, Runtime에 작성되거나 제공되는 xml에 대해서는 inflate를 적용할 수 없다. 다시말해 **`R.`으로 시작되는 resource 파일들만 inflate 가 가능하다.**

overload 하고 있는 inflate API함수들은 다음과 같다.
```
View inflate(int resource, ViewGroup root)
View inflate( XmlPullParser parser, ViewGroup root )
View inflate( XMLPullParser parser, ViewGroup root, boolean attachToRoot )
View inflate( int resource, ViewGroup root, boolean attachToRoot )
```

지금까지 다룬 `LayoutInflater` 외에도 `MenuInflater`도 있다. MenuInflater 는 `MenuInflater()`를 통해 얻어와서 (@activity) void inflate(int menuRes, Menu menu)를 호출한다. 이 MenuInflater도 LayoutInflater과 마찬가지로 성능상의 문제 때문에 `R.`으로 시작되는 resource 파일들에 대해서만 inflate가능하다. 