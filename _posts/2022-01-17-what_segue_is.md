---



layout: post



title: Segue 연결



subtitle: 실습으로 segue 배우기



categories: Swift



tags: [swift]



---

# Segue 에 대하여 배워보자



우선 Controller 안에 CocoaTouchClass 파일을 만들어보겠습니다.

그리고 Main.storyboard 로 이동하여서 해당 클래스파일과 연결 시켜줄 스크린을 클릭 후

![connectScreenandfile.gif](https://github.com/halaxhenry/halaxhenry.github.io/blob/main/assets/images/connectScreenandfile.gif?raw=true)

연결 시켜줄 클래스파일을 선택해줍니다.



그리고 Main.storyboard 로 가서 스크린 간의 segue를 연결하기위해

이동 후 

![segueConnectionbtwController.gif](https://github.com/halaxhenry/halaxhenry.github.io/blob/main/assets/images/segueConnectionbtwController.gif?raw=true)

control + click 을 통해 컨트롤러 간에 연결시켜줍니다.



그리고 segue를 Trigger 하기 위해선 Identifier 또한 정해줘야합니다.

![setIdentifierforsegue.png](https://github.com/halaxhenry/halaxhenry.github.io/blob/main/assets/images/setIdentifierforsegue.png?raw=true)



Segue 를 실행하기 위해 segue 의 Trigger 역할을 해줄 function 안에 performSegue 를 Call 합니다

```swift
class CalculateViewController: UIViewController {
    @IBOutlet weak var heightLabel: UILabel!
    @IBOutlet weak var weightLabel: UILabel!
    @IBOutlet weak var heightSlider: UISlider!
    @IBOutlet weak var weightSlider: UISlider!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func heightSliderChanged(_ sender: UISlider) {
        let height = String(format:"%.2f", sender.value)
        heightLabel.text = "\(height)m"
    }
    
    @IBAction func weightSliderChanged(_ sender: UISlider) {
        let weight = String(format:"%.0f", sender.value)
        weightLabel.text = "\(weight)Kg"
    }
    
    @IBAction func calculatePressed(_ sender: UIButton) {
        let height = heightSlider.value
        let weight = weightSlider.value
        
        let bmi = weight / pow(height, 2)
        
        self.performSegue(withIdentifier: "goToResult", sender: self)
      //self -> current CalculateViewController
    }
}
```



위에 보다시피 버튼을 통해 segue 를 trigging 할 것이므로 안에 performSegue 를 넣었습니다.

한번 시험삼아 실행해보겠습니다

![segueWorks.gif](https://github.com/halaxhenry/halaxhenry.github.io/blob/main/assets/images/segueWorks.gif?raw=true)



잘 이동하는 것을 볼 수 있겠쥬!!!



그리고 데이터를 전송할 controller 안에 받을 데이터를 담을 Optional 변수 하나를 만들어준다.

```swift
class ResultViewController: UIViewController {
    
    var bmiValue: String?

// 편의를 위해 밑 부분은 생략
```



Segue 하기 위해 아래의 템플릿 코드를 참고하기 바란다.

```swift
    /*
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
    }
    */
```

![prepareforsegue](/Users/seungchulha/Developer/gitBlog/halaxhenry.github.io/assets/images/prepareforsegue.gif)



연결 후 prepare 함수 안에 segue.identifier 가 일치할 경우 어떻게 행동할지를 설정해줄꺼에요

```swift
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "goToResult" {
            let destinationVC = segue.destination

        }
    }
```



근데 여기서 문제가 하나 발생합니다

destinationVC.bmiValue 라고 했는데 못찾는 상황이 발생하죠

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  if segue.identifier == "goToResult" {
    let destinationVC = segue.destination
    destinationVC.bmiValue = "1"
  }
}
// 반환된 에러 : Value of type 'UIViewController' has no member 'bmiValue'
```

이유는 segue.destination 은 UIViewConroller 를 데이터 타입으로 받습니다.

그러므로  destinationVC 또한 UIViewController 인거죠 

우리는 UIViewController 에서 우리가 쓸 ResultViewController 로 변환 시켜줘야할 필요가 있습니다요.

이 경우 를 위해서 우리는 특별한 처리를 해주어야합니다

```swift
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "goToResult" {
            let destinationVC = segue.destination as! ResultViewController
            destinationVC.bmiValue = "1"
        }
    }
```

이제 Error free 네요!!

위에서 사용한 as 는 Downcasting 이라고 부릅니다

! 표시를 한 이유는 forced 로 downcast 하겠다는 의미를 갖습니다

downcast 란 상위 클래스가 아닌 하위 클래스를 상속 받겠다는 의미인것같아요

UIViewController 자체가 아닌 UIViewController 를 Super class 로 삼은 우리가 만든 ResultViewController 를 받는 거죠!!



자 그렇다면 이제 계산된 bmi 의 값을 다음 화면으로 던져주는 작업을 해야겠죠!!

```swift
import UIKit

class CalculateViewController: UIViewController {
    
    var bmiValue = "0.0"
    
    @IBOutlet weak var heightLabel: UILabel!
    @IBOutlet weak var weightLabel: UILabel!
    @IBOutlet weak var heightSlider: UISlider!
    @IBOutlet weak var weightSlider: UISlider!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func heightSliderChanged(_ sender: UISlider) {
        let height = String(format:"%.2f", sender.value)
        heightLabel.text = "\(height)m"
    }
    
    @IBAction func weightSliderChanged(_ sender: UISlider) {
        let weight = String(format:"%.0f", sender.value)
        weightLabel.text = "\(weight)Kg"
    }
    
    @IBAction func calculatePressed(_ sender: UIButton) {
        let height = heightSlider.value
        let weight = weightSlider.value
        
        let bmi = weight / pow(height, 2)
        bmiValue = String(format: "%.1f", bmi)
        
        self.performSegue(withIdentifier: "goToResult", sender: self)
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "goToResult" {
            let destinationVC = segue.destination as! ResultViewController
            destinationVC.bmiValue = bmiValue
        }
    }
}
```

함수와 함수 사이에 변수를 던져주기 위해 함수 밖에 bmiValue라는 변수를 새로 지정해주었습니다 

다음 화면에서 String으로 받아야하기때문에 위의 bmiValue 또한 String 값으로 초기값을 정해주었고

calculatePressed 함수 안에서 bmi 값을 String 변환 하여 던져줍니다.

그러면 다음과 같이 값을 받는 것을 볼수 있죠 

```swift
class ResultViewController: UIViewController {
    
    var bmiValue: String?
    
    @IBOutlet weak var bmiLabel: UILabel!
    @IBOutlet weak var adviceLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        bmiLabel.text = bmiValue // 받은 bmiValue 를 label 에서 보여주기!!!
        
    }
```



이제 segue에 대해 어느정도 설명이 된거같습니다

그렇다면 결과 화면에서 어떻게 첫 화면 으로 돌아갈까요? 

간단합니다!!

```swift
    @IBAction func recalculatePressed(_ sender: UIButton) {
        self.dismiss(animated: true, completion: nil)
    }

```

dimiss 를 통해 modal 로 뜬 화면을 사라지게 할 수 있습니다.