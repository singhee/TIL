# 모델 레이어
뷰 객체는 사용자 인터페이스를 구성한다. 따라서 개발자는 인터페이스 빌더에서 뷰 객체를 만들고 설정하고 연결한다. 반면 모델 레이어는 직접 코드로 생성한다.

```swift
class ViewController: UIViewController {

    @IBOutlet weak var questionLabel: UILabel!
    @IBOutlet weak var answerLabel: UILabel!
    
    let questions: [String] = ["From what is congnac made?",
                               "What is 7+7?",
        "What is the capital of Vermont?"]
    let ansewers: [String] = ["Grapes",
                             "14",
							 "Montpelier"]
    var currentQuestionIndex: Int = 0
							
							.
							.
							.							
}
```
위의 코드에서 배열들은 `let`으로 선언한 반면 정수는 `var`로 선언한 것에 주목하자. 
**상수**는 `let` 으로 표시하고 그 값은 변경할 수 없다. 반면 **변수**는 `var` 로 표시하고 그 값은 변경할 수 있다.

## 액션 메소드 구현하기
```swift
...
@IBAction func showNextQuestion(sender: AnyObject){
	currentQuestionIndex += 1
        if currentQuestionIndex == questions.count {
            currentQuestionIndex = 0
        }
        
        let question: String = questions[currentQuestionIndex]
        questionLabel.text = question
        answerLabel.text = "???"
}
    
@IBAction func showAnswer(sender: AnyObject){
	 let answer: String = answers[currentQuestionIndex]
        answerLabel.text = answer
}

```