---
title: "[SwiftUI] 사진을 찍지않는 카메라-1"

categories:
  - SwiftUI
tags:
  - [SwiftUI]
  
toc: true
toc_sticky: true

date: 2023-06-15
last_modified_at: 2023-06-15
---

## ◼︎ AI로 사진을 생성해주는 카메라가 있다?<br>
![chalkak2](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/11d1db89-3910-4fe3-ae13-1670a6cbc1e4)

[파라그라피카(Paragraphica)](https://bjoernkarmann.dk/project/paragraphica)는 AI로 사진을 생성해주는 카메라다. 외관은 일반 카메라처럼 생겼지만 빛을 모아주는 렌즈 없이 빨간색 장식만 존재한다. 뒷면 디스플레이에는 텍스트가 적혀있는데 이는 네트워크를 통해 사용자의 **현재 장소나 시간, 날씨 등의 정보**를 가져온 것이다. 이와 같은 정보를 이용해 실시간으로 AI로부터 이미지를 생성한다.<br>파라그라피카로 생성한 이미지 예시는 다음과 같다.

![chalkak3](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/943c6c2d-69de-40e2-9c4d-4f7bd2a8d1f5)
![chalkak4](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/1733a075-eb93-4703-8676-0734a49f3e46)
[해당 사이트](https://camera.sandbox.noodl.app/))에서 웹으로 체험해볼 수 있지만 유료기도 하고 직접 만들어보면 어떨까 라는 생각이 들어 개발해보기로 했다.

## ◼︎ 과연 어떻게..??
나는 AI로 이미지를 생성하는 것이 아니라 사용자의 위치정보를 가져와 **구글의 스트리트뷰 API**를 사용해 해당 위치의 스트리트뷰 이미지를 보여주는 것으로 정했다. 그러기위해선 다음과 같은 것들이 필요했다.
> 1. 스트리트뷰 API
> 2. 카메라 레이아웃
> 3. 사용자 위치 사용 + 방위

