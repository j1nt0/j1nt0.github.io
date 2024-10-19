---
title: "[Project] 구름영감 출시 회고"

categories:
  - Project
tags:
  - [Project]

date: 2024-10-19
last_modified_at: 2024-10-19
---

> 안녕하세요, 진토입니다.<br>
지난 MC3에서 사진의 배경을 제거하는 기술을 사용하였습니다.<br>
그것을 활용해 구름을 수집하는 앱을 만들었습니다. 근데 귀여운 아이디어를 곁들인.<br>
이름하야 **구름영감**! <br>
자, 그럼 구름영감 회고 시작하겠습니다.
> 

# 🌱 구름영감 (2024.8.8-9.10)

### 1. 기획 - 내 눈에 띈 구름 하나

지난 챌린지에서 `Vision`프레임워크를 사용하여 촬영한 사진 속 옷과 배경을 분리하였다. 사물을 인식하는 정확도가 높아 활용하기 위한 아이디어가 마구마구 샘솟았다. 먼저, 사진 속 물체와 배경을 분리할 수 있기 때문에 무언가를 보관할 수 있게 하자는 생각이 들었다. <br><br>
<img src="https://github.com/user-attachments/assets/9d514db1-142d-4d46-b6cb-142546db48de" align="center" width="66%"><br><br>
무엇을 보관할 수 있을까를 생각하자 곤충, 사람, 장갑, 구름, 옷차림 등이 떠올랐다. 고민을 하며 올려다본 하늘엔 새하얗고 포근한 구름이 떠다니고 있었다. 영감님을 덧붙이는 것은 오래 걸리지 않았다. 어떤 구름들은 풍성한 영감님들의 수염과 비슷해 보였기 때문이다. 그렇게 구름영감님의 도망간 수염을 찾는 컨셉이 완성되었다. 맑은 하늘의 소장하고 싶은 구름을 촬영하고 그 구름들을 모아 구름영감님의 수염으로 장식하는 기능을 지닌 앱이라고 소개할 수 있겠다.

### 2. 디자인 - 영감님, 아프지말고 늘 행복하세요

처음 떠올렸던 영감님은 스크루지 영감처럼 고약한 성격의 밉상이지만 뭔가 부탁을 한다면 거절할 수 없는 매력의 소유자처럼 표현하고 싶었다. 그러나 풍성했던 수염을 도둑맞았다는 점에서 순진하면서 엉뚱한 느낌으로 표현하는게 더 적합할 것 같았다. 추가로 풍성한 수염에 비해 한올도 없는 머리카락으로 구름영감에게 수염의 필요성을 강조했다.<br><br>
<img src="https://github.com/user-attachments/assets/b0ad2327-7f53-4e80-bf47-98f4a7b0bae8" align="center" width="32%"><br>
<br>
가장 신경을 쓴 것은 앱을 처음 실행하고 만나게 되는 온보딩 페이지이다. 나에게는 귀여운 아이디어이지만 구름과 수염의 연관관계가 유저에게는 직관적이지 않을 수 있어 온보딩을 통해 스토리를 자연스럽게 받아들이게 하고싶었다. 그래서 마치 게임 속 캐릭터가 말을 하는 것처럼 구현하였다. <br>
기본적인 동작은 앱 내에서 `카메라`로 구름을 촬영하여 `수염 보관함`에서 직접 영감님의 수염을 붙여볼 수 있다. 구름을 많이 수집할수록 영감님의 수염이 풍성해진다. 촬영한 구름과 더불어 함께 꾸밀 수 있는 `스킨` 같은 장식적인 요소를 더했다. 나만의 방식으로 꾸민 구름영감은 `공유`기능을 통해 친구들에게 공유하거나 저장할 수 있다.

### 3. 개발 - Yo soy el Sabio de las Nubes.

구조 설계는 유지보수성을 고려하여 UI와 비즈니스 로직의 분리하는 `MVVM 패턴`을 적용하였다.<br>
`Model`: 애플리케이션의 데이터 구조를 정의하며, 비즈니스 로직 및 네트워크 통신 등을 처리합니다.<br>
`View`: 사용자 인터페이스(UI) 요소를 구성하며, 사용자와 상호작용하는 역할을 합니다.<br>
`ViewModel`: View와 Model 사이의 중개자 역할을 하며, UI와 관련된 데이터를 가공하고 관리합니다. View에서 발생하는 이벤트를 처리하고, Model에서 데이터를 가져와 View에 전달합니다.<br>
데이터 저장방식은 `SwiftData`를 이용하여 수집하는 구름과 스킨들을 저장하였다. `SwiftData`는 SwiftUI와 긴밀하게 통합되어 있어 데이터 작업을 쉽게 할 수 있다는 장점이 있지만 iOS 17버전 이상의 기기에서만 사용 가능하다는 단점이 있다.<br>
앱 동작의 기본적인 로직은 다음과 같다.<br>
`BeardTrack` - 카메라를 사용해 구름을 촬영하고 `Vision`프레임워크를 이용해 불필요한 배경을 제거한다. 촬영된 사진에 따라 배경을 제거하지 못할 수도 있다. 이 경우에도 수염으로 사용할 수 있게 하였지만 정상 촬영된 구름과 결과화면에 차이를 두어 구현하였다. 촬영된 모든 사진은 결과화면에서 저장할 지를 선택할 수 있다.<br>
`BeardOverview` - 촬영된 구름을 저장할 경우 수염 보관함에 위치한다. 수염들을 선택하면 화면에 띄워지고 이를 `DragGesture`를 통해 움직임을 추가해 영감님의 얼굴에 나만의 방식으로 수염을 붙일 수 있다. 수염들의 위치를 초기화할 수 있고 각각 또는 전부 보관함에서 삭제할 수 있다.<br>
`BeardDesign` - 스킨과 같은 장식요소로 영감님을 꾸민다. 추후 다양한 스킨을 추가하고 해금시키는 요소로 발전시킬 계획이다.<br>
`Share` - 나만의 방식으로 꾸민 영감님을 공유하기 위해 `ShareLink`를 사용했다. `ShareLink`는 SwiftUI에서 콘텐츠를 공유할 수 있는 기능을 제공하는 구조체로 다양한 데이터를 쉽게 다른 앱이나 소셜 미디어로 공유할 수 있다.<br>
앱을 만든다면 한국 뿐 아닌 더 많은 유저를 만나고 싶다는 생각을 해왔다. iOS 앱에서 로컬라이징 구현을 위해 `Strings File` 혹은 `String Catalog`가 존재하고 나는 새로 생긴 `String Catalog` 방식을 사용하였다. `String Catalog` 파일이 생성된 상태에서 프로젝트를 빌드하면 Xcode에서 프로젝트의 텍스트들을 인식해서 키값을 자동으로 생성해준다. 자동으로 생성되지 않은 키값은 변수 및 파라미터의 타입을 `LocalizedStringKey`로 수정하여 빌드하면 자동으로 생성된다. 한국어를 기본 언어로 하고 영어와 스페인어의 로컬라이징을 진행하였다.<br>
<img src="https://github.com/user-attachments/assets/52c11be9-e6c2-43c2-8b7f-d716dee1b27f" align="center" width="66%"><br>
구현이 완료된 앱을 Xcode에서 `Archive`하여 App Store Connet로 옮겨 심사를 진행한다. 앱 홍보 이미지를 첨부하고 앱 정보를 입력하여 제출하면 심사 대기 상태가 된다. 하루 정도가 흘러 심사가 승인되지 않았다는 메일이 왔다. 앱 스토어 등록이 까다롭다는 이야기를 들어서 첫 시도에 큰 기대를 하지 않았다. 그럼에도 감사했던 점은 승인되지 않은 사유를 굉장히 상세하게 제공해주었다는 것이다. 예를 들어, 유저에게 카메라 권한을 받을 때의 이유를 자세히 적으라는 등의 사유였다.    

### 4. 결과 - 기획과 디자인을 명징하게 개발해낸 신랄하면서 처연한 구름영감
<img src="https://github.com/user-attachments/assets/4c46123b-2dc5-4be6-a6fd-8e1c16bb405b" align="center" width="32%"><br><br>
미승인 사유들을 수정하여 다시 심사를 신청했고 이틀이 되지않아 승인이 되었다는 메일이 왔다. 곧 바로 앱 스토어에서 내가 만든 앱을 확인할 수 있었다. 처음부터 끝까지 혼자 작업해 출시한 경우는 처음이었기에 구름영감은 내게 애착이 많이 가는 앱이다. 출시와 함께 끝이 나는 게 아니라 이제부터는 유저와 만날 수 있다는 것이 더욱이 기대가 되는 점이다. <br>
<img src="https://github.com/user-attachments/assets/f89cf327-d32d-4a53-a665-efb7916c6cc3" align="center"><br>
구름영감을 기획부터 디자인, 개발, 그리고 출시에 이르기까지의 과정은 큰 도전이자 배움의 연속이었다. 다양한 문제를 해결하며 기능을 구현하는 동안 실력과 자신감이 함께 성장했음을 느꼈다. 사용자 입장에서 더 나은 경험을 제공하기 위해 여러 번 수정하고 개선하면서 끈기와 창의성의 중요성을 다시금 깨달았다. 이 앱을 통해 얻은 모든 지식과 경험은 앞으로의 프로젝트에도 큰 자산이 될 것이라고 생각한다.

### 5. 이후 - 사용자 경험과 앱의 수익화의 상관관계
현재 기본으로 제공되고 있는 스킨의 종류를 더 다양화하여 사용자들이 선택의 폭을 넓힐 수 있도록 하고자 했다. 이를 통해 `인앱결제 시스템`을 도입하여 프리미엄 스킨을 구매할 수 있는 옵션을 제공하거나, `영상 시청 광고`를 통해 무료로 잠금 해제할 수 있는 기능을 추가하여 앱의 수익을 창출하고자 했다. 이렇게 함으로써 사용자 경험을 높이는 동시에 앱의 수익 구조를 강화하려고 했다.<br>
<p align="center">
<img src="https://github.com/user-attachments/assets/cad24f22-b930-4121-8d71-0e378755003e" align="center" width="32%">
<img src="https://github.com/user-attachments/assets/b2c2bd7e-07b5-487b-9006-d043c9eae9e1" align="center" width="32%">
</p>
<br> 
기존 방식은 `영상 시청 광고`를 통해 스킨을 개수만큼만 잠금 해제할 수 있어 광고 시청 횟수에 제한이 있었다. 이를 개선하기 위해 광고를 보면 포인트를 획득하고, 그 포인트로 스킨을 잠금 해제하는 방식으로 변경하였다. 포인트 획득 시 랜덤 요소를 추가해 여러 번 시도하도록 하여 광고 수익을 증대하였다. 또한, 광고만 제공하는 것이 사용자에게 부담이 될 수 있어, 매일 출석할 때 무료 포인트를 제공하는 시스템을 추가하였다.<br>
앱 스토어 바로가기 - [‎구름영감](https://apps.apple.com/kr/app/%EA%B5%AC%EB%A6%84%EC%98%81%EA%B0%90/id6636475497)