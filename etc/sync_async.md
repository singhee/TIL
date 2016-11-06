# Synchronous(동기) vs Asynchronous(비동기)

### Synchronous (동기)
	 
	동기는 동시에 일어난다는 말이다. "요청과 그 결과가 동시에 일어난다는 약속" 이라고도 말할 수 있다. 
	요청을 하면 그 요청한 자리에서 바로 결과가 주어져야 한다는 말이다. 
	동기 방식은 시간이 얼마나 걸리든 요청한 그 자리에서 결과를 주겠다는 약속이다. 
	즉, 요청과 결과가 한자리에서 동시에 일어난다는 말이다.


### Asynchronous (비동기)
	 
	비동기는 동기와는 반대로 동시에 일어나지 않는다는 의미이다. 
	"요청과 그 결과가 동시에 일어나지 않을거라는 약속" 이라고도 말할 수 있다. 
	즉, 요청한 그 자리에서 바로 결과가 주어지는 것이 아니라, 나중에 결과를 주겠다는 약속이다. 


#### 동기와 비동기의 구분 이유
동기와 비동기를 구분하는 이유는 상황에 따라 장단점이 있기 때문이다. 동기방식은 설계가 간단하고 직관적이지만, 결과가 주어질 때까지 아무것도 못하고 대기해야 하는 단점이 있다. 비동기방식은 좀 더 복잡하지만 결과가 주어지는 시간이 길어져도 그 시간 동안 다른 작업을 할 수 있으므로 좀 더 효율적으로 자원을 사용할 수 있는 장점이 있다.
