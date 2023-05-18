---
title: "[SwiftUI] Category Tab - Custom TabBar"

categories:
  - SwiftUI
tags:
  - [SwiftUI]
  
toc: true
toc_sticky: true

date: 2023-05-17
last_modified_at: 2023-05-17
---

앱을 개발하다 보면 각 개체를 성질 혹은 항목에 따라 분류하는 탭이 필요할 때가 있습니다.
예를 들어 상점 앱을 구현할 때에 각 상점은 카테고리를 갖고 이를 아래와 같이 분류할 수 있을 겁니다.



사용한 Firebase의 데이터는 아래와 같습니다.
<img width="1132" alt="스크린샷 2023-04-29 오전 1 25 22" src="https://user-images.githubusercontent.com/124751277/235202166-51e86a24-73e2-4f9d-8179-432558f28600.png">

## • Model
상점들의 데이터를 담으므로 `Store.swift` 파일을 생성합니다.

```swift
// Store.swift
import Foundation

struct Store: Identifiable {
    
    var id: String
    var name: String
    var number: String
    var address: String
    var category: String
    var menu: Dictionary<String, Int>
    var operatingHour: Dictionary<String, String>
    var position: Array<Double>
    
}
```
Firebase에서 저장한 데이터들을 Model에서 구조화합니다.

Store의 인스턴스를 생성할 경우, 각각의 개체를 구별하기 위해 **Identifiable 프로토콜**을 추가해줍니다.
**Identifiable 프로토콜**은 id가 필수이며 id는 문서 추가 시 배정되는 ID값을 사용하게 됩니다.

또한 Firebase의 `맵`타입과 `배열`타입은 Swift에서 각각 `Dictionary`타입과 `Array`타입으로 사용합니다.

## • ViewModel
데이터를 처리할 `ViewModel.swift` 파일을 생성합니다.

뷰와 **데이터바인딩**을 수행하기 위해 **ObservableObject 프로토콜**을 추가합니다.
이후 변수에는 **@Published 속성**으로 관찰 대상 변수를 선언합니다.

```swift
// ViewModel.swift
import Foundation
import Firebase

class ViewModel: ObservableObject {
    
    @Published var list = [Store]()
    
    func getData() {
        
        let db = Firestore.firestore()
        
        db.collection("shop").getDocuments { snapshot, error in
            
            if error == nil {
                // No errors
                
                if let snapshot = snapshot {
                    
                    // Update the list property in the main thread
                    DispatchQueue.main.async {
                        
                        self.list = snapshot.documents.map { d in
                            return Store(id: d.documentID,
                                         name: d["name"] as? String ?? "",
                                         number: d["number"] as? String ?? "",
                                         address: d["address"] as? String ?? "",
                                         category: d["category"] as? String ?? "",
                                         menu: d["menu"] as? [String:Int] ?? ["":0],
                                         operatingHour: d["menu"] as? [String:String] ?? ["":""],
                                         position: d["position"] as? [Double] ?? [0])
                        }
                    }
                }
            }
            else {
                // Handle the errors
            }
        }
    }
    
}
```
코드에서 사용된 `DispatchQueue`란 **비동기적 실행**을 위해 사용되는 것인데 조금 더 알아봐야 할 것 같다.

`snapshot`에 데이터를 받아온 후 변수 `list`에 담는 내용이다. `error`를 사용하여 예외처리도 가능하다.

## • View
```swift
// ContentView.swift
import SwiftUI
import Firebase

struct ContentView: View {
    
    @ObservedObject var model = ViewModel()
    
    var body: some View {
        List (model.list) { item in
            HStack {
                Text(item.name)
                    .font(.system(size: 20))
                Spacer()
                VStack(alignment: .trailing) {
                    Text(item.category)
                    Text(item.address)
                        .font(.system(size: 15))
                }
            }
        }
    }
    
    init() {
        model.getData()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

```
뷰에서는 앞서 `ObservableObject`을 사용한 객체를 `@ObservedObject` 속성과 함께 선언한다.
이로써 연결된 데이터가 변경되었을 때 화면을 다시 그릴 수 있게 해준다.  

데이터를 가져오는 것은 `init()`함수를 통해 프로그램 시작과 함께 `getData()`함수를 실행하여 가져온다.

SwiftUI에서 List를 사용할 때에는 데이터 간 식별이 가능해야 한다.
즉, `Identifiable` 프로토콜을 사용하지 않는 데이터를 사용할 경우 **런타임 에러**가 발생한다.
여기서는 앞서 모델에 `Identifiable`을 추가하였기에 `List`를 사용할 수 있다.

실행 화면은 다음과 같다.

<img width="270" alt="스크린샷 2023-04-29 오전 1 25 22" src="https://user-images.githubusercontent.com/124751277/235210088-1d8e8d84-f108-4eb1-b04f-6e189b8971e1.png">
