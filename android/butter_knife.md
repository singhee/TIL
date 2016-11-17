# 안드로이드 라이브러리 - Butter Knife(View Inject)
안드로이드에서 새로운 화면을 만들면서 xml레이아웃을 만든 뒤 제일먼저 하는 작업이 대부분은 해당 View 들을 선언하고 할당하는 작업이다. 

View가 10개라면 10번 선언, 10번 findViewById를 매번 해주어야 하는 번거로움이 있다. 이러한 번거로움을 해결해 주는 라이브러리가 Butter knife이다.

기본적인 사용방법은 아주 간단하다.

```
compile 'com.jakewharton:butterknife:7.0.1'	// Gradle 추가
```

### Bind
- Activity
```java
super.setContentView(layoutResID);
ButterKnife.bind(this);
```

- Fragment
```java
View view = LayoutInflater.from(getActivity()).inflate(resId, null);
ButterKnife.inject(this, view);
```

### View객체 선언
View를 선언해주면서 @Bind를 이용해 해당 View의 id를 할당해준다.
```
@Bind(R.id.view_action_sms)
View view_action_sns;

@Bind(R.id.view_action_comment)
View view_action_comment;

@Bind(R.id.et_comment)
CustomEditText et_comment;

@Bind(R.id.btn_comment_send)
SelphoneSendButton btn_comment_send;
```

### ClickListener 추가
`ClickListener` 가 필요한 경우 `setOnClickListener`를 선언해줄 필요 없이 바로 실행될 함수를 만들고 `@OnClick`을 사용하면 된다.
```java
@OnClick(R.id.view_action_call)
void onCallClick(){
	...
}
```

여러 뷰에 대해서 하나의 함수만을 실행시키고 싶다면 아래와 같이 선언해주면 된다.
```java
@OnClick({R.id.pager, R.id.indicator, R.id.cell_picture, R.id.cell_product})
public void onSomeThingClick(View view){
	...
}
```

### Resource Bind
xml에 만들어둔 resource값들도 바로 bind해서 가져올 수 있다.
지원하는 bind 는 다음과 같다.
```java
@BindString(R.string.title) String title;
@BindBool(R.bool.isTed) boolean isTed;
@BindInt(R.integer.myinteger) int myinteger;
@BindDrawable(R.drawable.graphic) Drawable graphic
@BindColor(R.color.red) int red;
@BindDimen(R.dimen.spacer) Float spacer;
@BindDimen(R.dimen.intvalue) int intvalue;
```
