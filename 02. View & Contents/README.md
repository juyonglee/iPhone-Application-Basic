# 02. View & Contents

## 1. View와 Contents
### View: Content를 출력
Content에 적합한 View를 사용

1. 문자 기반 Content
    - View: UILabel
    - Content: String
2. 이미지 기반 Content
    - View: UIImageView
    - Content: UIImage

## 2. 문자 기반의 Content
- View: UILabel
- Content: String
### 속성
1. **출력 Content**
    - Text
2. **굴자 색과 Font**
    - **Font**
        - Font 설정
        - Style 설정
            - Ex> Body, Headline
3. **Dynamic Text**
    - **`[설정-> 손쉬운 사용]`** 에서 사용자가 설정한 텍스트 크기에 따라 글자 크기가 변경 됨
    - Text Style로 설정한 경우에만 반영
4. **Content와 View의 Size**
    1. Label Size 및 Content Size
        - Font, Size, 글자 수로 size를 결정
        - `Size to Fit Content`를 이용하면 자동으로 content 크기에 맞춰 View의 size가 조정됨
    2. 문자열과 Label 크기가 다른 경우
        - Case 1. Label이 문자열보다 큰 경우
        
            문자열 위치를 조정하는 `Alignment`속성을 이용
    
        - Case 2. 문자열이 Label보다 큰 경우
            1. 글자 일부분 축약
                - **`Line Break`** 속성을 이용하여 `생략 위치 설정`
            2. 글자 size 조절
                - **`Autoshrink`** 속성을 이용
                - `최소 크기(Minimum Font Size) 및 비율(Minimum Font Scale)을 지정`
            3. 여러 줄로 표현
                - Lines 속성으로 설정
                - 줄을 바꾸는 방식을 설정 
                    - `Word Warp`
                    - `Character Wrap`
                - **`0`** 으로 설정하는 경우 크기에 맞춰서 줄을 표현

### Code로 Label 작성하기
- Class: **`UILabel`**
- UILabel의 Content
    ```swift
    var text: String?
    ```
    - [Example 1] Label의 Size를 명시하여 생성하는 Case
        ```swift
        let label = Label(frame: CGRECT(x: 50, y: 50, width: 250, height: 100))
        label.text = "Juyong Lee"
        self.view.addSubview(label)
        ```
    - [Example 2] Content로 Label size를 정하는 Case
        ```swift
        let label = Label(frame: CGRECT(x: 50, y: 50, width: 0, height: 0))
        label.text = "Juyong Lee"
        label.sizeToFit()
        self.view.addSubview(label)
        ```
1. Content 출력 설정
    - 정렬
        ```swift
        var textAlignment: NSTextAlignment 
        ```
    - **`문자열 생략`**
        ```swift
        var lineBreakMode: NSLineBreakMode
        ```
    - Auto shrink
        ```swift
        var adjustsFontSizeToFitWidth: Bool 
        var minimumScaleFactor: CGFloat
        ```
    - 문자열 줄 수
        ```swift
        var numberOfLines: Int
        ```
2. Font 설정
    - Class: **`UIFont`**
        ```swift
        var font: UIFont!
        ```
    - Font 객체 생성
        ```swift
        func systemFont(ofSize fontSize: CGFloat) -> UIFont 
        func boldSystemFont(ofSize fontSize: CGFloat) -> UIFont 
        init?(name fontName: String, size fontSize: CGFloat)
        ```
    - [Example] Font 생성
        ```swift
        let font1 = UIFont.systemFontOfSize(15)
        let font2 = UIFont(name: "Marker Felt", size: 20)
        ```
3. Dynamic Type 설정
    - Dynamic Type Font 생성 **`[주의! Font Style 설정]`**
        ```swift
        class func preferredFont(forTextStyle style: UIFontTextStyle) -> UIFont
        ``` 
    - [Example]
        ```
        label.font = UIFont.preferredFont(forTextStyle: .headline) 
        label.text = "Headline Style 적용"
        ```
4. Custom Font 
    - [Step01] 원하는 Font file을 Project에 추가한다.
    - [Step02] plist에 **`Fonts provided by application`** 에 Font명을 추가한다.
    - [Step03] Font 객체를 생성한다.
        ```swift
        label.font = UIFont(name: "SeoulHangang", size: 25)
        ```

## 3. 이미지 기반의 Content
- View: UIImageView
- Content: UIImage

### 이미지 Resource
- 기기의 적용 이미지 파일을 의미하며 이미지 이름에 배율을 입력하여 사용한다.
    ```
    [기기에 맞는 이미지가 자동으로 적용]
    Non-Retina Display Model: image.png
    Retina Display Model: image@2x.png
    Retina Display 5.5inch Model: image@3x.png
    ```
1. **App UI 이미지**
    - App과 함께 배포 `(설치하는 프로그램에 이미지들이 포함되어 있다.)`
    - Bundle이라는 App 배포 Package에 위치
2. **사용자 Content 이미지**
    - 이미지 라이브러리(앨범) 내의 이미지
    - 기기의 다운로드한 이미지
    - 네트워크를 이용하여 접근한 이미지
3. 이미지 리소스를 추가하는 방법
    1. APP과 함께 배포되는 이미지 Resource
        - `이미지 파일 단위`로 이미지 Resource 다루기
            1. Finder에서 이미지를 프로젝트 끌어 넣는 경우
                - Copy items
                - Create groups: Group 생성 `(즉, Folder 구조를 무시한다!)`
                - Create folder references: Folder 구조 유지 설정
                - Add to targets: Application Target 설정
            2. Add Files 메뉴를 이용하여 이미지를 추가하는 경우
        - `Asset`을 이용하여 이미지 Resource 다루기
            1. Asset: Project의 Resource를 관리
                - App Icon
                - Image Set: App과 함께 배포되는 이미지
                    - 배율 별 이미지 설정
                    - 이미지 Set 이름 설정
                    - 이미지 Set Group 설정
                - Launch Image
                - Data
                