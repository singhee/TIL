# App Bar

Android 에서 App Bar 가 하는 역할은 다음과 같다.
- 앱의 제목 및 사용자가 현재 앱 안의 어느 부분을 사용하고 있는지 안내한다.
- 사용자가 검색과 같은 주요 액션을 실행하는 창구이다.
- 네비게이션 및 뷰 전환 (Tab이나 Dropdown List를 사용해서)

`ActionBar`로 `App Bar`를 구현할 수도 있지만, 좀 더 다양한 기기에서 일관적인 기능/디자인을 보장하기 위해 **android.support.v7.widget.Toolbar**로 구현하는 것을 권장한다. Toolbar로 구현한 `App Bar`에는 v7 appcompat ActionBar 클래스의 기능을 적용할 수 있다. 
```
// app_bar_main.xml
<android.support.design.widget.AppBarLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:theme="@style/AppTheme.AppBarOverlay">

    <android.support.v7.widget.Toolbar
     	android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary"
        app:popupTheme="@style/AppTheme.PopupOverlay" />

</android.support.design.widget.AppBarLayout>
```

`Custom Toolbar`를 적용하기 위해선 매니페스트 Application 태그에 `NoActionBar`테마를 지정해야 한다. 그리고 액티비티 레이아웃 상단에 Toolbar태그를 추가한다. 그리고 해당 액티비티 onCreate 메서드에서 setSupportActionBar(Toolbar toolbar) 메서드를 통해 툴바 인스턴스를 액션바로 설정하는 과정이 필요하다.
```
// AndroidManifest.xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <activity
    	android:name=".MainActivity"
        android:label="@string/app_name"
        android:theme="@style/AppTheme.NoActionBar">
        <intent-filter>
         	<action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>

</application>
```

```
//MainActivity.java
protected void onCreate(Bundle savedInstanceState){
	super.onCreate(sacedInstanceState);
	...
	setSupportActionBar(toolbar);
	
	//...
}
```

이 과정을 끝내고 그대로 앱을 실행해보면 App Bar에 아무런 내용이 없다. 액션을 추가하기 위해서는 별도의 options menu 리소스 파일을 만들어서 menu item들을 정의한다. 이 때 효과적인 아이콘을 사용하도록 하고, `app:showAsAction`값은 ifRoom 또는 never를 권장한다. 그리고 액티비티 클래스의 onCreateOptionsMenu(Menu menu)에서 MenuInflater를 사용해 메뉴 리소스 파일을 inflate한다. 각각의 액션을 핸들링하기 위해선 onOptionItemSelected(MenuItem item)메서드를 오버라이드 한다. 선택된 MenuItem이 인자로 전달되면 item.getItemId()로 아이디를 식별하여 핸들링한다.

```
// MainActivity.java

   @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
    
   @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
    

```




