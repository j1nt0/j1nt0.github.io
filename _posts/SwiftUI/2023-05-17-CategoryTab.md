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

앱을 개발하다 보면 각 개체를 성질 혹은 항목에 따라 분류하는 탭이 필요할 때가 있습니다.<br>
예를 들어 상점 앱을 구현할 때에 각 상점은 카테고리를 갖고 이를 아래와 같이 분류할 수 있을 겁니다.

![예시 이미지](https://github.com/j1nt0/SwiftUI-Basic/assets/124751277/59d6ec6a-27ba-4ba5-b339-1041d7026bd7)

이를 구현하기 위해 다음과 같은 카테고리와 데이터를 사용하였습니다.<br>
```swift
enum Category: String {
    case a = "전체"
    case b = "한식"
    case c = "중식"
    case d = "패스트푸드"
    case e = "아시안"
    case f = "분식"
    case g = "카페"
}

struct Store {
    var name: String
    var category: Category
}

var stores: [Store] = [
    Store(name: "민들레한정식", category: .b),
    Store(name: "자금성", category: .c),
    Store(name: "햇볕팟타이", category: .e),
    Store(name: "미란다짬뽕", category: .c),
    Store(name: "우리동네떡볶이", category: .f),
    Store(name: "곰돌카페", category: .g),
    Store(name: "뜨거마라탕", category: .c)
]
```

카테고리의 변화를 감지하고 이를 반영하기 위해서는 **상태프로퍼티(State)**가 필요합니다.<br>
[State Property](https://developer.apple.com/documentation/swiftui/state)는 변화가 생기면 해당 변수의 값을 읽거나 쓸 수 있음을 의미하는 프로퍼티 래퍼입니다.
- State Property 생성을 위해선 `@State`를 사용하여 선언한다.
- SwiftUI는 State로 선언된 Property들의 저장소를 관리한다.
- State Property의 값이 변경되면 View를 다시 렌더링하기 때문에 항상 최신 값을 가진다.
- 다른 View에서 State Property를 참조하기 위해서는 `@Binding`을 사용해야 한다.

```swift
struct ContentView: View {
    
    // State Property 선언
    @State var selectedTab: Category = .a
    
    var body: some View {
        Text("Hello World")
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

`ScrollView`를 선언하고 내부에 카테고리 수만큼의 버튼을 생성합니다.<br>
`.horizontal` 속성을 사용하여 스크롤 방향을 가로로 변경해줍니다.
```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 40) {
                Button {
                    
                } label: {
                    Text("전체")
                }
                Button {
                    
                } label: {
                    Text("한식")
                }
                Button {
                    
                } label: {
                    Text("중식")
                }
                Button {
                    
                } label: {
                    Text("패스트푸드")
                }
                Button {
                    
                } label: {
                    Text("아시안")
                }
                Button {
                    
                } label: {
                    Text("분식")
                }
                Button {
                    
                } label: {
                    Text("카페")
                }
            }
        }
    }
}
```
<img width="263" alt="예시 이미지2" src="https://github.com/j1nt0/SwiftUI-Basic/assets/124751277/01c2b368-f1b6-4e6f-853e-ac26d6614bbb">

각 버튼을 클릭하면 해당하는 **State**로 변경하게 만들고 선택된 State에 따라 글자 두께와 색깔이 바뀌게 해줍니다. 
```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 40) {
                Button {
                    selectedTab = .a
                } label: {
                    Text("전체")
                        .fontWeight(selectedTab == .a ? .bold : .medium)
                        .foregroundColor(selectedTab == .a ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .b
                } label: {
                    Text("한식")
                        .fontWeight(selectedTab == .b ? .bold : .medium)
                        .foregroundColor(selectedTab == .b ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .c
                } label: {
                    Text("중식")
                        .fontWeight(selectedTab == .c ? .bold : .medium)
                        .foregroundColor(selectedTab == .c ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .d
                } label: {
                    Text("패스트푸드")
                        .fontWeight(selectedTab == .d ? .bold : .medium)
                        .foregroundColor(selectedTab == .d ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .e
                } label: {
                    Text("아시안")
                        .fontWeight(selectedTab == .e ? .bold : .medium)
                        .foregroundColor(selectedTab == .e ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .f
                } label: {
                    Text("분식")
                        .fontWeight(selectedTab == .f ? .bold : .medium)
                        .foregroundColor(selectedTab == .f ? .green : .secondary)
                        .font(.system(size: 20))
                }
                Button {
                    selectedTab = .g
                } label: {
                    Text("카페")
                        .fontWeight(selectedTab == .g ? .bold : .medium)
                        .foregroundColor(selectedTab == .g ? .green : .secondary)
                        .font(.system(size: 20))
                }
            }
        }
    }
}
```

<img width="263" alt="예시 이미지3" src="https://github.com/j1nt0/SwiftUI-Basic/assets/124751277/c1f6a0ff-78e1-4085-bb1b-e0c361483cab">

위의 코드를 **3개의 View**로 분할하겠습니다. 이 과정에서 앞서 말한 `@Binding`을 사용합니다.<br>
보기 좋게 정리했을 뿐, 결과는 위와 같습니다.
```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        CategoryTabBar(selectedTab: $selectedTab)
            // 앞쪽에만 padding을 줘, 뒷쪽에는 내용이 이어짐을 예상하게 합니다.
            .padding(.leading)
    }
}

struct CategoryTabBar: View {
    
    // @Binding 사용
    @Binding var selectedTab: Category
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 40) {
                CategoryTabBarButton(tab: .a, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .b, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .c, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .d, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .e, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .f, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .g, selectedTab: $selectedTab)
            }
        }
    }
}

struct CategoryTabBarButton: View {
    
    var tab: Category
    // @Binding 사용
    @Binding var selectedTab: Category
    
    var body: some View {
        Button {
            selectedTab = tab
        } label: {
            Text(tab.rawValue)
                .fontWeight(selectedTab == tab ? .bold : .medium)
                .foregroundColor(selectedTab == tab ? .green : .secondary)
                .font(.system(size: 20))
        }
    }
}
```

카테고리의 선택에 따라 아래 컨텐츠를 변경시키기 위해 `switch case`를 사용하겠습니다.
```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        VStack {
            CategoryTabBar(selectedTab: $selectedTab)
                .padding(.leading)
            // 카테고리 선택에 따른 아래 컨텐츠
            ScrollView(.horizontal, showsIndicators: false) {
                switch selectedTab {
                case .a:
                    Text("a")
                case .b:
                    Text("b")
                case .c:
                    Text("c")
                case .d:
                    Text("d")
                case .e:
                    Text("e")
                case .f:
                    Text("f")
                case .g:
                    Text("g")
                }
            }
        }
    }
}
```

각 case 별로 해당하는 카테고리의 Store만 보여주어야 합니다.<br>
이를 위해서는 ForEach를 사용하여 하나씩 해당하는 카테고리인지를 확인하고 걸러주는 방법을 사용하겠습니다.<br>  
당장 ForEach를 사용하게되면 다음과 같은 오류를 마주하게 됩니다.<br>
`'ForEach' requires that 'Store' conform to 'Identifiable'`<br>
ForEach를 사용하려는 개체는 `Identifiable` 속성이 요구됩니다. 초반에 생성한 Store 타입에 `Identifiable`을 추가하고 UUID()를 사용하여 각각의 개체에 유일성을 부여합니다.
```swift
struct Store: Identifiable {
    var id = UUID()
    var name: String
    var category: Category
}
```

카테고리 별로 ForEach를 통과시키게 합니다.<br>
`case .a:` 즉, "전체" 카테고리의 경우 모든 Store가 표시되고 `다른 case`들은 `if문`을 통해 현재 선택된 State의 값과 같은 카테고리를 갖는 Store만이 표시되게 합니다. 
```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        VStack {
            CategoryTabBar(selectedTab: $selectedTab)
                .padding(.leading)
            // 카테고리 선택에 따른 아래 내용란
            ScrollView(.horizontal, showsIndicators: false) {
                switch selectedTab {
                case .a:
                    HStack {
                        ForEach(stores) { store in
                            ZStack {
                                RoundedRectangle(cornerRadius: 10)
                                    .frame(width: 100, height: 200)
                                    .foregroundColor(.green)
                                Text(store.name)
                                ZStack {
                                    Capsule()
                                        .frame(width: 50, height: 25)
                                        .foregroundColor(.white)
                                    Text(store.category.rawValue)
                                }
                                .offset(x: 20, y: -80)
                            }
                        }
                    }
                case .b:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                case .c:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                case .d:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                case .e:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                case .f:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                case .g:
                    HStack {
                        ForEach(stores) { store in
                            if store.category.rawValue == selectedTab.rawValue {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 10)
                                        .frame(width: 100, height: 200)
                                        .foregroundColor(.green)
                                    Text(store.name)
                                    ZStack {
                                        Capsule()
                                            .frame(width: 50, height: 25)
                                            .foregroundColor(.white)
                                        Text(store.category.rawValue)
                                    }
                                    .offset(x: 20, y: -80)
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
```
<img width="263" alt="예시 이미지4" src="https://github.com/j1nt0/SwiftUI-Basic/assets/124751277/8d104b15-a371-4d4d-8843-fa0ddd01c2ac">

반복되는 코드를 2개의 뷰로 정리할 수 있다.

```swift
struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        VStack {
            CategoryTabBar(selectedTab: $selectedTab)
            // 카테고리 선택에 따른 아래 내용란
            StoreCards(selectedTab: $selectedTab)
        }
        .padding(.leading)
    }
}

struct StoreCards: View {
    
    @Binding var selectedTab: Category
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            switch selectedTab {
            case .a:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .b:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .c:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .d:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .e:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .f:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .g:
                StoreCardsHStack(selectedTab: $selectedTab)
            }
        }
    }
}

struct StoreCardsHStack: View {
    
    @Binding var selectedTab: Category
    
    var body: some View {
        LazyHStack(spacing: 10) {
            ForEach(stores) { store in
                // 현재 탭바의 카테고리가 전체 혹은 Store의 카테고리와 일치할 경우 출력
                if store.category.rawValue == selectedTab.rawValue || selectedTab.rawValue == "전체" {
                    ZStack {
                        RoundedRectangle(cornerRadius: 10)
                            .frame(width: 100, height: 200)
                            .foregroundColor(.green)
                        Text(store.name)
                        ZStack {
                            Capsule()
                                .frame(width: 50, height: 25)
                                .foregroundColor(.white)
                            Text(store.category.rawValue)
                        }
                        .offset(x: 20, y: -80)
                    }
                }
            }
        }

    }
}
``

[전체코드]
```swift
import SwiftUI

enum Category: String {
    case a = "전체"
    case b = "한식"
    case c = "중식"
    case d = "패스트푸드"
    case e = "아시안"
    case f = "분식"
    case g = "카페"
}

struct Store: Identifiable {
    var id = UUID()
    var name: String
    var category: Category
}

var stores: [Store] = [
    Store(name: "민들레한정식", category: .b),
    Store(name: "자금성", category: .c),
    Store(name: "햇볕팟타이", category: .e),
    Store(name: "미란다짬뽕", category: .c),
    Store(name: "우리동네떡볶이", category: .f),
    Store(name: "곰돌카페", category: .g),
    Store(name: "뜨거마라탕", category: .c)
]

struct ContentView: View {
    
    @State var selectedTab: Category = .a
    
    var body: some View {
        VStack {
            CategoryTabBar(selectedTab: $selectedTab)
            // 카테고리 선택에 따른 아래 내용란
            StoreCards(selectedTab: $selectedTab)
            Spacer()
        }
        .padding(.leading)
    }
}

struct CategoryTabBar: View {
    
    // @Binding 사용
    @Binding var selectedTab: Category
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 40) {
                CategoryTabBarButton(tab: .a, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .b, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .c, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .d, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .e, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .f, selectedTab: $selectedTab)
                CategoryTabBarButton(tab: .g, selectedTab: $selectedTab)
            }
        }
    }
}

struct CategoryTabBarButton: View {
    
    var tab: Category
    // @Binding 사용
    @Binding var selectedTab: Category
    
    var body: some View {
        Button {
            selectedTab = tab
        } label: {
            Text(tab.rawValue)
                .fontWeight(selectedTab == tab ? .bold : .medium)
                .foregroundColor(selectedTab == tab ? .green : .secondary)
                .font(.system(size: 20))
        }
    }
}

struct StoreCards: View {
    
    @Binding var selectedTab: Category
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            switch selectedTab {
            case .a:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .b:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .c:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .d:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .e:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .f:
                StoreCardsHStack(selectedTab: $selectedTab)
            case .g:
                StoreCardsHStack(selectedTab: $selectedTab)
            }
        }
    }
}

struct StoreCardsHStack: View {
    
    @Binding var selectedTab: Category
    
    var body: some View {
        LazyHStack(spacing: 10) {
            ForEach(stores) { store in
                // 현재 탭바의 카테고리가 전체 혹은 Store의 카테고리와 일치할 경우 출력
                if store.category.rawValue == selectedTab.rawValue || selectedTab.rawValue == "전체" {
                    ZStack {
                        RoundedRectangle(cornerRadius: 10)
                            .frame(width: 100, height: 200)
                            .foregroundColor(.green)
                        Text(store.name)
                        ZStack {
                            Capsule()
                                .frame(width: 50, height: 25)
                                .foregroundColor(.white)
                            Text(store.category.rawValue)
                        }
                        .offset(x: 20, y: -80)
                    }
                }
            }
        }

    }
}


struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

```
