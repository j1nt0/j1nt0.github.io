---
title: "[SwiftUI] Custom Tab Bar"

categories:
  - SwiftUI
tags:
  - [SwiftUI, Custom TabBar]

date: 2023-04-07
last_modified_at: 2023-04-07
---

앱을 사용하다보면 우리는 다양한 **Tab bar**를 보게된다.
![](https://velog.velcdn.com/images/0000_0010/post/55daed96-c85b-42cf-9173-fbfb4408c34e/image.png)

SwiftUI에서는 **TabView**를 통해 쉽게 구현할 수 있다.
```swift
//TabView
struct ContentView : View {
  var body: some View {
    TabView {
      Text("The First Tab")
        .tabItem {
          Image(systemName: "1.square.fill")
        }
      Text("The Second Tab")
        .tabItem {
          Image(systemName: "2.square.fill")
        }
      Text("The Third Tab")
        .tabItem {
            Image(systemName: "3.square.fill")
        }
    }
  }
}
```
**<div align="center">[SwiftUI의 TabView]</div>**

<center><img src = "https://velog.velcdn.com/images/0000_0010/post/dffad987-57fb-4413-b988-1f6a75703c42/image.GIF"></center>

하지만 내가 만들고자 하는 Tab Bar를 구현하기 위해서는 많은 수정이 불가피했다.
[다음 동영상](https://www.youtube.com/watch?v=v19fln0e_qQ)을 참고하여 **Custom Tab Bar**를 구현해보았다.

```swift
//Custom TabView
enum Tab {
    case first
    case second
    case third
}

struct ContentView : View {
    @State var selectedTab: Tab = .first
    var body: some View {
        VStack {
            Spacer()
            switch selectedTab {
            case .first:
                Text("The First Tab")
            case .second:
                Text("The Second Tab")
            case .third:
                Text("The Third Tab")
            }
            Spacer()
            CustomTabView(selectedTab: $selectedTab)
        }
    }
}

struct CustomTabView: View {
    @Binding var selectedTab: Tab
    var body: some View {
        HStack {
            Spacer()
            Button() {
                selectedTab = .first
            } label: {
                Image(systemName: "1.square.fill")
            }
            Spacer()
            Button {
                selectedTab = .second
            } label: {
                Image(systemName: "2.square.fill")
            }
            Spacer()
            Button {
                selectedTab = .third
            } label: {
                Image(systemName: "3.square.fill")
            }
            Spacer()
        }
    }
    
}
```
**<div align="center">[Custom TabView]</div>**

<center><img src = "https://velog.velcdn.com/images/0000_0010/post/15112b0f-7ca2-48ba-8a7b-e17c79979a42/image.GIF"></center>

ZStack을 사용해 동그란 버튼을 만들고 offset으로 위치를 변경하였다.
```swift
struct CustomTabView: View {
    @Binding var selectedTab: Tab
    var body: some View {
        HStack {
            Spacer()
            Button() {
                selectedTab = .first
            } label: {
                Image(systemName: "1.square")
            }
            Spacer()
            ZStack {
                Circle()
                    .foregroundColor(Color(.orange))
                    .frame(width: 90, height: 90)
                Button {
                    selectedTab = .second
                } label: {
                    Image(systemName: "2.square")
                }
            }
            .offset(y: -32)
            Spacer()
            Button {
                selectedTab = .third
            } label: {
                Image(systemName: "3.square")
            }
            Spacer()
        }
    }
}
```
<center><img src = "https://velog.velcdn.com/images/0000_0010/post/7f53be7d-322c-4f6a-933a-0075cf977633/image.GIF"></center>


Tab Bar의 배경은 도형을 그리는 것이 익숙치 않아 이미지로 넣어주었고 색상은 Assets에 "Symbol"로 지정하여 사용하였다.
몇가지를 추가로 수정한 코드와 결과는 다음과 같다.
```swift
struct CustomTabView: View {
    
    @Binding var selectedTab: Tab
    
    var body: some View {
        ZStack {
            Image("background")
                .resizable()
                .aspectRatio(contentMode: .fill)
                .offset(y: 15)
            HStack {
                Spacer()
                Button {
                    selectedTab = .home
                } label: {
                    Image(systemName: selectedTab == .home ? "house.fill" : "house")
                        .resizable()
                        .aspectRatio(contentMode: .fit)
                        .frame(width: 30)
                        .foregroundColor(.white)
                }
                Spacer()
                Button {
                    selectedTab = .shop
                } label: {
                    ZStack {
                        Circle()
                            .foregroundColor(Color("Symbol"))
                            .frame(width: 90, height: 90)
                        Image(systemName: selectedTab == .shop ? "basket.fill" : "basket")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                            .frame(width: 40)
                            .foregroundColor(.white)
                    }
                    .offset(y: -32)
                }
                Spacer()
                Button {
                    selectedTab = .profile
                } label: {
                    Image(systemName: selectedTab == .profile ? "person.fill" : "person")
                        .resizable()
                        .aspectRatio(contentMode: .fit)
                        .frame(width: 30)
                        .foregroundColor(.white)
                }
                Spacer()
            }
        }
        .frame(height: 50)
    }
}
```
<center><img src="https://velog.velcdn.com/images/0000_0010/post/7d2320d3-9e26-4928-a736-e16efc056c61/image.GIF"></center>
