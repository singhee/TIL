# [Firebase - Read and Write Data on Android](https://firebase.google.com/docs/database/android/retrieve-data?hl=ko)

## Get a DatabaseReference
Firebase로 부터 데이터를 검색을 하기 위해서는 `DatabaseReference`에 대한 instance 가 필요하다. 즉, `DatabaseReference`에 비동기 리스너를 연결하여 리스너가 데이터의 초기 상태가 확인될 때 한 번 호출된 후 데이터가 변경될 때마다 다시 호출된다.

```java
private DatabaseReference mDatabase;
// ... 
mDatabase = FirebaseDatabase.getInstance().getReference();
``` 

데이터를 검색은 다음과 같은 유형의 이벤트를 수신 대기할 수 있다.

리스너 | 이벤트 콜백 | 일반적인 용도
------------------|-----------------------|-------------------------------
valueEventListener| onDataChanged()| 경로의 전체 내용에 대한 변경을 읽고 수신 대기한다. 
ChildEventListener| onChildAdded()| 항목 목록을 검색하거나 항목 목록에 대한 추가를 수신 대기한다. onChildChanged() 및 onChildRemoved()와 함께 사용하여 목록의 변경을 모니터링할 수 있다.
 | onChildChanged()|목록의 항목에 대한 변경을 수신 대기한다. onChildAdded() 및 onChildRemoved()와 함께 사용하여 목록의 변경을 모니터링할 수 있다.
  | onChildRemoved()| 목록의 항목 삭제를 수신 대기한다. onChildAdded() 및 onChildChanged()와 함께 사용하여 목록의 변경을 모니터링할 수 있다.
   | onChildMoved() | 순서 있는 데이터에 사용하여 항목 우선순위의 변경을 수신 대기한다. 

값 EventListener를 추가하려면 `addValueEventListener()`또는 `addListenerForSingleValueEvent()`메소드를 사용한다. 하위 EventListener를 추가하려면 `addChildEventListener()`메소드를 사용한다. 


## 값 이벤트
`onDataChange()` 메소드를 사용해 이벤트 발생 지점을 기준으로 지정된 경로에 있는 내용의 Static Snapshot을 읽을 수 있다. 이 메소드는 리스너가 연결될 때 한 번 호출된 후 하위를 포함한 데이터가 변경될 때마다 다시 호출된다. 하위 데이터를 포함하여 해당 위치의 모든 데이터를 포함하는 Snapshot이 이벤트 콜백에 전달된다. 데이터가 없는 경우 반환되는 Snapshot은 `null`이 된다. 

다음 예제에서는 데이터베이스에서 게시물 세부정보를 검색하는 소셜 블로깅 애플리케이션을 보여준다.
```
ValueEventListener postListener = new ValueEventListener(){
	@Override
	public void onDataChange(DataSnapshot dataSnapshot){
		// Get Post object and use the values to update the UI
		Post post = dataSnapshot.getValue(Post.class);
		// ...
	}

	@Override
	public void onCancelled(DatabaseError databaseError){
		// Getting Post failed, log a message
		Log.w(TAG, "loadPost:onCancelled", databaseError.toException());
		// ...
	}	
};
mPostReference.addValueEventListener(postListener);
```
리스너는 이벤트 발생 시점에 데이터베이스에서 지정된 위치에 있던 데이터를 포함하는 `DataSnapshot`을 수신한다. 스냅샷에 대해 `getValue()`를 호출하면 데이터를 나타내는 Java개체 표현이 반환된다. 지정된 위치에 데이터가 없는 경우 `getValue()`를 호출하면 `null`이 반환된다.

위의 예제에서 `ValueEventListener`는 읽기가 취소된 경우에 호출되는 `onCancelled()`메소드도 정의한다. 예를 들어 클라이언트에 Firebase 데이터베이스 위치에서 데이터를 읽을 권한이 없으면 읽기가 취소될 수 있다. 이 메소드에는 실패 이유를 나타내는 `DatabaseError`개체가 전달된다. 


## 하위 이벤트 (onChildAdded, onChildChanged, onChildRemoved, onChildMoved, onCancelled)


`onChildAdded()` 콜백은 일반적으로 Firebase 데이터베이스의 항목 목록을 검색하는 데 사용된다. `onChildAdded()` 콜백은 기존 하위 항목마다 한 번씩 발생한 후 지정된 경로에 하위 항목이 새로 추가될 때마다 다시 발생한다. 리스너에는 새 하위 항목의 데이터를 포함하는 Snapshot이 전달된다. 

`onChildChanged()` 콜백은 하위 노드가 수정될 때마다 호출된다. 여기에는 하위 노드의 하위에 대한 수정이 포함된다. 이 콜백은 일반적으로 `onChildAdded()` 및 `onChildRemoved()`콜백과 함께 항목 목록의 변경에 대응하는 데 사용된다. 이벤트 리스너에게 전달되는 Snapshot에는 하위 항목의 업데이트된 데이터가 포함된다. 

`onChildRemoved()` 콜백은 바로 아래 하위 항목이 삭제될 때 호출된다. 이 콜백은 일반적으로 `onChildAdded()` 및 `onChildChanged()`콜백과 함께 사용된다. 이벤트 콜백에 전달되는 Snapshot에는 삭제된 하위 항목의 데이터가 포함된다.

하위 항목의 재정렬을 야기하는 업데이트에 의해 `onChildChanged()` 콜백이 호출될 때마다 `onChildMoved()`콜백이 호출된다. 이 콜백은 우선순위별로 정렬된 데이터와 함께 사용된다.  

## 데이터 한 번 읽기
한 번만 호출되고 즉시 삭제되는 콜백이 필요한 경우가 있다. 이후에 변경되지 않는 UI요소를 초기화할 때가 그 예이다. 이러한 경우 `addListenerForSingleValueEvent()` 메소드를 사용하면 된다. 이 메소드는 한 번 호출된 후 다시 호출되지 않는다.

이 방법은 한 번 로드된 후 자주 변경되지 않거나 능동적으로 수신 대기할 필요가 없는 데이터에 유용하다. 예를 들어 위 예제의 블로깅 앱에서는 사용자가 새 게시물을 작성하기 시작할 때 이 메소드로 사용자의 프로필을 로드한다.

```
final String userId = getUid();
mDatabase.child("users").child(userId).addListenerForSingleValueEvent(
	new ValueEventListener() {
		@Override
		public void onDataChange(DataSnapshot dataSnapshot){
			// Get user Value
			User user = dataSnapshot.getValue(User.Class);
			
			// ...
		}

		@Override
		public void onCancelled(DatabaseError databaseError){
			Log.w(TAG, "getUser:onCancelled", databaseError.toException());
			// ...
		}
});

```













