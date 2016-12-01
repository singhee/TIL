# [Firebase - Realtime Database 의 데이터 구조화](https://firebase.google.com/docs/database/android/structure-data)
Firebase의 실시간 데이터베이스의 JSON 데이터 구조화와 관련된 주요 데이터 아키텍처 개념 및 권장사항을 알아보고자 한다. 

## 데이터의 구조 : JSON트리
모든 Firebase Realtime Database 의 데이터는 **JSON 개체**로 저장된다. SQL 데이터베이스와 달리 테이블이나 레코드가 없으며, JSON 트리에 추가된 데이터는 기존 JSON구조의 노드로 들어가게 된다. 

## 데이터 구조 권장사항
#### 데이터 중첩 배제 
Firebase 실시간 데이터베이스는 최대 32단계의 데이터 중첩을 허용히므로 기본 구조에 중첩을 도입해도 괜찮다고 생각할 수 있다. 그러나 데이터베이스의 특정 위치에서 데이터를 가져오면 모든 하위 노드가 함께 검색된다. 또한 사용자에게 데이터베이스의 특정 노드에 대한 읽기 또는 쓰기 권한을 부여하면 해당 노드에 속한 모든 데이터에 대한 권한이 함께 부여된다. 따라서 실제 구현에서는 데이터 구조를 최대한 **평면화**하는 것이 좋다. 

다음은 데이터 중첩을 배제해야 하는 이유를 보여 주는 다중첩 구조의 예이다.
```
{
 "chats" : {
	"one" : {
		"title" : "Historical Tech Pioneers",
		"messages" : {
			"m1" : { "sender" : "ghopper", "message" : "Relay malfunction found. Cause: moth."},
			"m2" : { ... },
		}
	},
	"two" : { ... }
 }
}

```
위 같은 중첩 설계에서는 전체 데이터 반복이 어렵다. 예를 들어 채팅 대화 제목을 나열하려면 모든 회원과 메세지를 포함하여 전체 `chats`트리를 클라이언트에 다운로드해야 한다.

#### 데이터 구조 평면화
위의 데이터 중첩에 대한 문제를 해결하기 위한 방법으로는, 비정규화를 통해 데이터를 서로 다른 경로로 분할하는 것이다. 그러면 필요에 따라 별도의 호출을 통해 효율적으로 다운로드할 수 있다. 아래는 평면화된 구조의 예이다.

```
{
  "chats": {
    "one": {
      "title": "Historical Tech Pioneers",
      "lastMessage": "ghopper: Relay malfunction found. Cause: moth.",
      "timestamp": 1459361875666
    },
    "two": { ... },
    "three": { ... }
  },

  "members": {
    // we'll talk about indices like this below
    "one": {
      "ghopper": true,
      "alovelace": true,
      "eclarke": true
    },
    "two": { ... },
    "three": { ... }
  },

  "messages": {
    "one": {
      "m1": {
        "name": "eclarke",
        "message": "The relay seems to be malfunctioning.",
        "timestamp": 1459361875337
      },
      "m2": { ... },
      "m3": { ... }
    },
    "two": { ... },
    "three": { ... }
  }
}
```
이제 대화당 몇 바이트만 다운로드하여 대화방 목록 전체를 반복하면서 메타데이터를 가져와 UI에 대화방을 나열하거나 표시할 수 있다. 메세지가 도착할 때 별도로 메세지를 가져와서 표시할 수 있으므로 UI가 빠른 반응 속도를 유지한다. 


#### 확장 가능한 데이터 만들기 
앱을 개발할 때는 (특히 수천 개의 레코드가 목록에 포함되어 있을 때) 일부만 다운로드하는 것이 좋은 경우가 많다. 이 관계가 static이며 one-directional인 경우에는 상위 개체 아래에 하위 개체를 중첩하면 간단히 해결된다. 

경우에 따라서는 이 관계가 동적이거나 데이터를 비정규화해야 할 수 있다. 많은 경우 쿼리를 사용하여 데이터의 일부를 검색하면 데이터를 비정규화 할 수 있다.

이 방법도 충분하지 못할 수 있다. 사용자와 그룹 간의 양방향 관계를 예로 들 수 있다. 사용자는 그룹에 속할 수 있으며, 그룹은 사용자의 목록으로 구성된다. 이때, 사용자가 어떠한 그룹에 속할지를 결정하려면 문제가 다소 복잡해진다. 

이러한 경우 특정 사용자가 속하는 그룹을 나열하고 해당 그룹의 데이터만 가져오는 방법이 필요하다. 이때 그룹 **index**가 상당한 도움이 될 수 있다. 

```
{
  "users": {
    "alovelace": {
      "name": "Ada Lovelace",
      // Index Ada's groups in her profile
      "groups": {
         // the value here doesn't matter, just that the key exists
         "techpioneers": true,
         "womentechmakers": true
      }
    },
    ...
  },
  "groups": {
    "techpioneers": {
      "name": "Historical Tech Pioneers",
      "members": {
        "alovelace": true,
        "ghopper": true,
        "eclarke": true
      }
    },
    ...
  }
}
```
위와 같이 Ada의 레코드와 그룹 모두에 관계를 저장하면 일부 데이터가 중복됨을 알 수 있다. alovelace는 그룹 아래에 indexed 되었고  techpioneers는 Ada의 프로필에 나열되었다. 따라서 Ada를 그룹에서 삭제하려면 두 위치에서 업데이트가 이루어져야 한다. 

이러한 중복성은 양방향 관계에서 불가피하다. 사용자 또는 그룹 목록이 수백만으로 늘어나거나 실시간 데이터베이스 보안 규칙으로 인해 일부 레코드에 액세스할 수 없더라도 이 중복성 덕분에 Ada의 소속 그룹을 빠르고 효율적으로 확인할 수 있다.

이와 같이 ID를 키로 나열하고 값을 true로 설정하여 데이터를 반전하는 방식을 사용하면 키를 확인할 때 /users/$uid/groups/$group_id를 읽어서 null인지 여부만 확인하면 된다. **index**는 데이터보다 쿼리 또는 스캔 속도가 빠르고 훨씬 효율적이다. 











