---
title: "[SwiftUI] How to Hide Tab Bar"

categories:
  - SwiftUI
tags:
  - [SwiftUI]

date: 2023-04-10
last_modified_at: 2023-04-10
---

[커스텀 탭바](https://velog.io/@0000_0010/SwiftUI-Custom-Tab-Bar)를 통해 보여줄 페이지를 생성하였다.
각 페이지의 상단에는 NavigationLink를 통해 새로운 페이지로 넘어갈 수 있게 하였다.
<center><img src="https://velog.velcdn.com/images/0000_0010/post/ef5e67e6-ddad-4100-b26e-4d93cf3b304c/image.GIF"></center>

그러나, 새로운 페이지가 실행돼도 하단의 탭바가 사라지지 않고 존재했다.
원인을 찾아보니 NavigationStack의 위치 때문에 발생한 것이었다.
[해당 사이트](https://github.com/TreatTrick/Hide-TabBar-In-SwiftUI)를 참고하였다.

<center><img src = "https://velog.velcdn.com/images/0000_0010/post/f0cc676a-6faa-4447-b3d5-866f7dcab5ce/image.png" width="250px"></center>

기존 구조는 NavigationView()가 NavigationLink의 바로 상위에서 감싸고 있었다. 이는 새로운 페이지로 이동하여도 하단의 CustomTabView가 보이게 되는 사태를 초래했다.

<center><img src = "https://velog.velcdn.com/images/0000_0010/post/2abdff04-e6b2-4d63-b289-800e8a072779/image.png" width="250px"></center>

수정한 구조는 NavigationView()가 가장 상위단계에 위치하게 된다. 이를 통해 새로운 페이지로 이동할 시 CustomTabView가 보이지 않게 된다.

<center><img src ="https://velog.velcdn.com/images/0000_0010/post/712f66c6-4731-45bb-87cf-2018258d19ee/image.GIF"></center>
