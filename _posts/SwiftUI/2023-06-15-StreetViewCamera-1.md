---
title: "[SwiftUI] 사진을 찍지않는 카메라-1"

categories:
  - SwiftUI
tags:
  - [SwiftUI, StreetView Camera]
  
toc: true
toc_sticky: true

date: 2023-06-15
last_modified_at: 2023-06-15
---

## ◼︎ AI로 사진을 생성해주는 카메라가 있다?<br>
![chalkak2](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/11d1db89-3910-4fe3-ae13-1670a6cbc1e4)

[파라그라피카(Paragraphica)](https://bjoernkarmann.dk/project/paragraphica)는 AI로 사진을 생성해주는 카메라다. 외관은 일반 카메라처럼 생겼지만 빛을 모아주는 렌즈 없이 빨간색 장식만 존재한다. 뒷면 디스플레이에는 텍스트가 적혀있는데 이는 네트워크를 통해 사용자의 **현재 장소나 시간, 날씨 등의 정보**를 가져온 것이다. 이와 같은 정보를 이용해 실시간으로 AI로부터 이미지를 생성한다.
![chalkak3](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/943c6c2d-69de-40e2-9c4d-4f7bd2a8d1f5)
파라그라피카로 생성한 이미지 예시는 다음과 같다.
[해당 사이트](https://camera.sandbox.noodl.app/)에서 웹으로 체험해볼 수 있지만 유료기도 하고 직접 만들어보면 어떨까 라는 생각이 들어 개발해보기로 했다.

## ◼︎ 과연 어떻게..??
나는 AI로 이미지를 생성하는 것이 아니라 사용자의 위치정보를 가져와 **구글의 스트리트뷰 API**를 사용해 해당 위치의 스트리트뷰 이미지를 보여주는 것으로 정했다. 그러기 위해선 다음과 같은 것들이 필요했다.
> 1. 스트리트뷰 API
> 2. 카메라 레이아웃
> 3. 사용자 위치 사용

## ◼︎ 그전에..
![chalkak7](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/201ac135-625f-4ad0-b50c-07a2982ebd8f)
카메라 앱이기 때문에 가로모드를 기준으로 개발할 것이다. 앱을 가로모드로 사용할 수 있게 하려면 `Xcode > General > Deployment Info > iPhone Orientation`을 `Landscape Right`로 설정해준다. 

## ◼︎ 스트리트뷰 API
![chalkak5](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/c75ead66-d8b2-4ad2-aea4-d18bb46ea4a4)
먼저, 스트리트뷰 API를 사용하기 위해선 Google Maps Platform에서 API키를 생성해야 한다.

![chalkak6](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/a7296c4e-a642-43bd-a1df-862f8959f6e5) 
우리는 **Street View Static API**를 사용할 것이므로 검색해서 사용 설정을 해준다.

```swift
struct StreetView: View {
    let apiKey: String
    let latitude: Double
    let longitude: Double

    var body: some View {
        if let streetViewURL = URL(string: "https://maps.googleapis.com/maps/api/streetview?size=400x250&location=\(latitude),\(longitude)&fov=80&key=\(apiKey)"),
           let imageData = try? Data(contentsOf: streetViewURL),
           let streetViewImage = UIImage(data: imageData) {
            Image(uiImage: streetViewImage)
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: 600)
        } else {
            Text("Failed to load Street View")
        }
    }
}

struct ContentView: View {
    var body: some View {
        VStack {
            StreetView(apiKey: "YOUR_API_KEY", latitude: 37.5373140468913ㅋ, longitude: 126.978466063237)
        }
    }
}
```
<img width="600" alt="스크린샷 2023-06-15 오후 3 45 13" src="https://github.com/j1nt0/j1nt0.github.io/assets/124751277/54b066e4-e930-4d8f-813a-7954c877315d">
SwiftUI에서 원하는 위치의 스트리트뷰 이미지를 가져올 수 있게 됐다.

## ◼︎ 카메라 레이아웃
명색이 카메라 앱인데 몇가지 기능이 필요하다 생각했다. 
> 1. 셔터 버튼
> 2. 플래시라이트 버튼
> 3. 리셋 버튼
> 4. 저장 버튼
> 5. 격자 프레임 

### 1. 셔터 버튼
기존 퍼런 화면에서 스트리트뷰 이미지로 변경하는 버튼이다. 디자인은 아이폰의 기본카메라 버튼처럼 구현하였다.<br>
셔터 버튼 클릭 시, 현재 촬영상태 변수인 `isTakeFicture`을 `true`로 변경한다. 상태는 아래에 나올 리셋 버튼에서 다시 `false`로 변경된다.
```swift
struct ShutterButton: View {
    
    @Binding var isTakeFicture: Bool
    
    var body: some View {
        Button {
                isTakeFicture = true
            }
        } label: {
            ZStack{
                Circle().foregroundColor(.white).frame(width: 80)
                Circle().stroke(Color.white, lineWidth: 5).frame(width: 95)
            }
        }
    }
}
```

### 2. 플래시라이트 버튼
```swift
class FlashlightManager: ObservableObject {
    private let captureDevice = AVCaptureDevice.default(for: AVMediaType.video)
    
    @Published var isFlashlightOn = false
    
    init() {
        guard let device = captureDevice else { return }
        if device.hasTorch {
            do {
                try device.lockForConfiguration()
                device.torchMode = .off
                device.unlockForConfiguration()
            } catch {
                print("Failed to access flashlight.")
            }
        }
    }
    
    func toggleFlashlight() {
        guard let device = captureDevice else { return }
        if device.hasTorch {
            do {
                try device.lockForConfiguration()
                device.torchMode = isFlashlightOn ? .off : .on
                device.unlockForConfiguration()
                isFlashlightOn.toggle()
            } catch {
                print("Failed to access flashlight.")
            }
        }
    }
}

struct FlashButton: View {
    
    @StateObject private var flashlightManager = FlashlightManager()
    
    var body: some View {
        Button {
            flashlightManager.toggleFlashlight()
        } label: {
            Image(systemName: "bolt.circle")
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: 30)
                .foregroundColor(.white)
        }
    }
}
```

### 3. 리셋 버튼
카메라를 다시 찍기위해 기존 퍼런 화면 상태로 되돌려 주는 버튼이다. 셔터 버튼의 반대 역할로 `isTakeFicture`을 `false`로 변경한다
```swift
struct ResetButton: View {
    
    @Binding var isTakeFicture: Bool
    
    var body: some View {
        Button {
            isTakeFicture = false
        } label: {
            Image(systemName: "arrow.triangle.2.circlepath")
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: 40)
                .foregroundColor(.white)
        }
    }
}
```

### 4. 저장 버튼
찍은 사진을 저장할 수 있는 버튼이다.
사진을 저장하는 기능은 공부를 통해 다음 포스팅에 실도록 하겠다. 

### 5. 격자 프레임
사실 없어도 무방한 기능이지만 카메라스러움을 살려주기에 아주 찰떡인 요소이다.
```swift
struct GuideLine: View {
    
    var body: some View {
        HStack {
            Spacer()
            Rectangle()
                .frame(width: 0.8)
                .foregroundColor(.white)
            Spacer()
            Rectangle()
                .frame(width: 0.8)
                .foregroundColor(.white)
            Spacer()
        }
        VStack {
            Spacer()
            Rectangle()
                .frame(height: 0.8)
                .foregroundColor(.white)
            Spacer()
            Rectangle()
                .frame(height: 0.8)
                .foregroundColor(.white)
            Spacer()
        }
    }
}
```

<img width="600" alt="스크린샷 2023-06-15 오후 4 10 50" src="https://github.com/j1nt0/j1nt0.github.io/assets/124751277/4709b8d8-abe7-4271-8957-f79ee67532ad">

다음 포스팅에선 **사용자 위치**를 반영해 같은 위치의 스트리트뷰 이미지를 띄우는 것을 해보겠습니다. 실제 카메라가 바라보는 방향의 이미지를 가져오려면 **방위 데이터**도 함께 반영하는 것이 좋겠습니다. 마지막으로 사진을 저장하는 것도 시도해보겠습니다.  
