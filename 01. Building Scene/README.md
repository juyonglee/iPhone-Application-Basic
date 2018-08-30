# Building Scene

## 1. Scene - iOS App에서의 화면 단위
### UIKit Framework: User Interface 작성을 위한 요소를 제공

### Scene 구성 요소
1. View Controller
    - Content 출력용 View: `UIView`
    - 사용자 입력이 가능한 Control: `UIControl`
    - Scene 제어: `UIViewController`
2. View
    
    Content를 표시
### Scene 좌표계
1. 기기의 크기와 해상도
    - iPhone 3: 3.5 인치, 320 * 480
    - iPhone 4 (Retina Display): 3.5 인치, 640 * 960
2. Point 좌표 이용: 왼쪽 상단 부터 (0, 0)
    - iPhone 3: (10, 10) point -> (10, 10) pixcel
    - iPhone 4: (10, 10) point -> (20, 20) pixcel
### Scene 작성
1. Interface Builder로 작성
    - Story Board에 Scene작성        
        - Library에서 View 배치
        - View 구조 조절
        - View의 속성 설정
    - `동적으로 변경 불가능`
2. Code로 작성
    - `동적으로 변경 가능`

### Scene의 생명주기
```swift
//  View 구조 다루기 적당한 생명 주기
func viewDidLoad()

func viewWillAppear(_ animated: Bool) 

func viewWillLayoutSubviews()

//  Layout이 반영된 이후
func viewDidLayoutSubviews()

func viewDidAppear(_ animated: Bool) 

func viewWillDisappear(_ animated: Bool) 

func viewDidDisappear(_ animated: Bool)
```
## 2. View: UIView
### View의 위치와 크기
- frame: `위치와 크기` `[CGRect 구조체]`
- origin : 위치 `[CGPoint 구조체]`
- center : 중심 좌표 `[CGPoint 구조체]`
- size : 크기 `[CGSize 구조체]`
### View 구조
- Scene에 View를 배치해 View 구조를 생성한다
- `[Superview - Subview]` 관계
- `Document outline에서 아래 위치한 View가 상위에 나타난다`

### View 속성
Xcode의 View Inspector
1. File : 스토리보드 파일 속성
2. Quick Help : 항목 도움말
3. Identity : Class, ID, IB내 이름 
4. Attributes : 항목 속성
    - Content 채우기 모드
    - Tag
    - Touch Event 
    - 투명도
    - 배경색 숨기기
5. Size : 크기
    - Frame: x, y, width, height 
    - 제약조건
    - Content Hugging, Compression
6. Connections : 연결 정보

### Code로 View 생성
1. View 생성
    ```swift
    //  1. 위치와 크기 정보 입력
    let frame = CGRect(x: 10, y: 10, width: 100, height: 100) 
    let view1 = UIView(frame:frame)

    //  2. 뷰 생성 후 frame으로 위치/크기 입력
    let view = UIView()
    view.frame = CGRect(x: 10, y: 10, width: 100, height: 100)
    ```
2. View의 색상, 투명도
- 색깔 : UIColor
- RGBA : 0 ~ 1.0 사이의 값
    ```swift
    var tintColor: UIColor!
    var backgroundColor: UIColor?
    
    //  View의 배경색 바꾸기
    view.backgroundColor = UIColor.red
    view.backgroundColor = UIColor(red: 1.0, green: 0.0, blue: 1.0, alpha: 1.0)
    ```
3. View의 구조
- Scene의 `Root View`
    - UIViewController
        ```swift
        var view: UIView!
        ```
- `UIView 클래스`
    ```swift
    var superview: UIView? { get } 
    var subviews: [UIView] { get }
    ```
    1. 자식 View 추가
        ```
        func addSubview(_ view: UIView)
        ```
    2. Scene에 View 추가하기
        ``` swift
        let childView = UIView(frame: CGRect(x: 10, y: 10, width: 100, height: 100)) 
        self.view.addSubview(childView)
        ```
    3. View 구조 중간에 삽입
        ```swift
        func insertSubview(_ view: UIView, at index: Int)
        ```
    4. 씬에 뷰 추가하기
        ```swift
        func bringSubview(toFront view: UIView)
        func sendSubview(toBack view: UIView)
        ```
### Code로 View 제어
1. View의 Tag
    ```swift
    var tag: Int
    ```
    - Tag로 찾기 
        ``` swift
        func viewWithTag(_ tag: Int) -> UIView?
        ```
    - recursive, 자식 View까지 검색
        ```swift
        override func viewDidAppear(animated: Bool) {
            if let view99 = self.view.viewWithTag(99) {
                view99.backgroundColor = UIColor.gray
            }
        }
        ```
2. Outlet
- View와 Code(Property) 연결
- Property에 @IBOutlet 지시자
- Outlet 작성 다이얼로그
    - Connection : Outlet 연결 
    - Object : ViewController 
    - Name : redView Property
    - Type : UIView 타입
    - Storage : Weak (ARC)
        ```swift
        class ViewController: UIViewController { 
            @IBOutlet weak var redView: UIView!
        }
        ```
- Storyboard에서 ViewController 선택 후 오른쪽 버튼 클릭해서 연결 가능
- Outlet 프로퍼티와 ARC
    1. 코드로 작성한 Property : strong
        - strong: 화면에서 삭제 -> Property 소유
    2. 인터페이스 빌더로 작성 : weak
        - weak: 화면에서 삭제 ->객체 해제 
- Outlet 연결 오류시 발생하는 메세지
    ```
    *** Terminating app due to uncaught exception 'NSUnknownKeyException', reason: '[<ViewController 0x6aae150> setValue:forUndefinedKey:]: this class is not key value coding-compliant for the key imageView.’
    ```
