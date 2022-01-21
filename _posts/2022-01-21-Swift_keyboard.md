

---







layout: post







title: Swift 키패드 (키보드) 활용하기 기초







subtitle: UITextFieldDelegate







categories: Swift







tags: [swift]







---

# SwiftUI Keyboard

UITextField 를 유저가 터치할 경우 화면 하단에 키패드가 뜨는 것을 시뮬레이션 하기 위해 시뮬레이터에서 간단한 설정을 해주어야합니다.

![image-20220121113823881](/Users/seungchulha/Library/Application Support/typora-user-images/image-20220121113823881.png)



Xcode에서 시뮬레이터를 실행할 경우에는 물리적 키보드 (우리가 타자를 치는 키보드) 로도 타입이 가능하지만

장비 ( 아이폰, 아이패드 등등 ) 에서는 유저가 터치를 통해 Typing 하기 때문에 이렇게 세팅을 하고 직접 유저 입장에 가까워져야할 필요가 있을 것 같아요



<hr>

위에 inputField 에 타이핑 한 결과물이 잘 나오는 지 확인해 봐야 할 필요가 있겠죠

위의 돋보기 버튼을 클릭하면 InputField 에 적은 것을 출력해서 확인해볼께요

```swift
import UIKit

class WeatherViewController: UIViewController, UITextFieldDelegate {

    @IBOutlet weak var conditionImageView: UIImageView!
    @IBOutlet weak var temperatureLabel: UILabel!
    @IBOutlet weak var cityLabel: UILabel!
    @IBOutlet weak var searchTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }

    @IBAction func searchPressed(_ sender: UIButton) {
        print(searchTextField.text!)
    }
}

// result : 
// go (input field 에 go 라고 쳤어요 ㅋㅋ) 
```



위에서 볼 수 있듯이 UITextFieldname.text 가 해당 필드의 값을 담고 있다는 것을 알 수 있죠잉!!

textfield가 empty 일 경우 default 값을 ""로 던져주기에 force wrapping 하였답니다.



<hr>

물론 위의 돋보기 버튼을 눌러서 결과를 확인할 수도 있지만 때로는 그것마저 귀찮을 수 있습니다.

그럴경우 타이핑 후 바로 엔터 버튼을 눌러 결과를 볼수 있도록 편의성을 제공해야할 필요가 있겠네요!!!

그래서 키보드 내에 있는 버튼을 통해 검색할 수 있도록 해주는 것을 세팅해보겠습니다.



우선 UITextFieldDelegate 를 해당 컨트롤러에 넣어줍니다

UITextFieldDelegate 프로토콜을 추가해주는 이유는 UITextField 를 다양한 방법으로 활용하기 위해서 입니다.

UITextFieldDelegate 는 키보드 입력 및 전반적인 text Field 편집과 관련된 기능들을 수행하는 프로토콜이라고 하네요

```swift
class WeatherViewController: UIViewController, UITextFieldDelegate {
```



그리고 viewDidLoad 안에 

```swift
override func viewDidLoad() {
        super.viewDidLoad()
        
        searchTextField.delegate = self
    }
```

searchTextField.delegate = self 를 넣어줍니다.

textField.delegate = self 의 의미는 textField 에 입력한 값을 VC에 전달하겠다는 뜻입니다.

```swift
    @IBAction func searchPressed(_ sender: UIButton) {
        searchTextField.endEditing(true)
        print(searchTextField.text!)
    }
    
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        searchTextField.endEditing(true)
        print(searchTextField.text!)
        return true
    }
```

위와 같이 우리는 두가지 방법으로 타이핑후 검색버튼을 실행할 수 있게되었습니다.

위에는 돋보기 버튼을 누를 시 검색

아래는 키패드 안에 있는 Return 버튼을 통해 검색 !!!



그리구 !! 위의 textField.endEditing(true) 는

수정이 끝난후 키패드가 사라지도록 (숨겨지도록) 하는 명령어에요!! 자세한건 위의 코드를 참고하세요!!

<hr>

```swift
func textFieldDidEndEditing(_ textField: UITextField) {
  			// 우선 이곳에 검색창이 공백이 되기전에 실행되야할 함수를 채워 넣어야겠군용!!!
  
        searchTextField.text = ""
    }
```

위의 코드는 내가 검색할 것을 타입후 검색버튼을 누르면 위의 input 창이 빈칸으로 변하도록 도와주는 역할을 합니다.



<hr>

유저가 타이핑 한 것에 대한 유효성을 확인하기위해 우리는 textFieldShouldEndEditing method를 사용할 수 있어요

만약 유저가 inputField 가 공백인 상태 혹은 유효하지 않은 것을 적고 Return 하는 것을 방지 해줄 수있는 유용한 메소드입니다.

```swift
    func textFieldShouldEndEditing(_ textField: UITextField) -> Bool {
        if textField.text != "" {
            return true
        } else {
            textField.placeholder = "Type something"
            return false
        }
    }
```

위와같이 조건문을 통해 유효성을 테스트합니다