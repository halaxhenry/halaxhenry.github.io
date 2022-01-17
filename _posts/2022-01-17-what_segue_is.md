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

