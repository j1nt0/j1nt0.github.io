---
title: "[SwiftUI] 사진을 찍지않는 카메라-2"

categories:
  - SwiftUI
tags:
  - [SwiftUI, StreetView Camera]
  
toc: true
toc_sticky: true

date: 2023-07-10
last_modified_at: 2023-07-10
---

## ◼︎ 사용자의 위치 가져오기<br>
사용자의 현재 위치를 사용하기 위해선 info.plist 파일의 수정이 필요하다.<br>
key를 `Privacy - Location When In Use Usage Description`로 설정하고 value를 위치 권한을 받아오기 위한 안내메세지로 적는다.

```swift
class LocationManager: NSObject, ObservableObject, CLLocationManagerDelegate {
    private let locationManager = CLLocationManager()

    @Published var latitude: Double = 0.0
    @Published var longitude: Double = 0.0

    override init() {
        super.init()
        self.locationManager.delegate = self
        self.locationManager.requestWhenInUseAuthorization()
        self.locationManager.startUpdatingLocation()
    }

    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        guard let location = locations.last else {
            return
        }
        latitude = location.coordinate.latitude
        longitude = location.coordinate.longitude
    }
}
```
다음은 사용자의 위치를 가져오는 코드이다.

## ◼︎ 방위 가져오기
사용자가 카메라로 사물을 찍는 방향은 실제 기기가 인식하는 방향과 다르다.
![무제-2](https://github.com/j1nt0/j1nt0.github.io/assets/124751277/25819892-a291-4559-a692-f7dcf9cd149c)
그렇기에 90을 더해주고 360으로 모듈러 연산을 진행한다.
```
class CompassManager: NSObject, ObservableObject, CLLocationManagerDelegate {
    private let locationManager = CLLocationManager()
    
    @Published var heading: Double = 0.0
    
    override init() {
        super.init()
        self.locationManager.delegate = self
        self.locationManager.startUpdatingHeading()
    }
    
    func locationManager(_ manager: CLLocationManager, didUpdateHeading newHeading: CLHeading) {
        self.heading = newHeading.trueHeading
    }
}

~~~
StreetView(isTakePicture: $isTakeFicture, apiKey: "api-key", latitude: locationManager.latitude, longitude: locationManager.longitude, heading: ***(Int(compassManager.heading)+90) % 360***)
```
