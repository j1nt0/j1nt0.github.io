---
title: "[SwiftUI] Multipeer Connectivity on iOS"

categories:
  - SwiftUI
tags:
  - [SwiftUI]

date: 2024-11-11
last_modified_at: 2024-11-11
---

## 🦧 Mulitpeer Connectivity란?

**Multipeer Connectivity**는 Apple에서 제공하는 프레임워크로, Bluetooth, Wi-Fi 및 Wi-Fi Direct와 같은 다양한 연결 기술을 사용하여 **근거리 통신**을 가능하게 한다. 이 프레임워크를 사용하면 여러 장치가 서로 직접 연결되어 **데이터를 주고받을 수 있다.** 중요한 점은 이 기능이 별도의 **인터넷 연결 없이도 동작**한다는 점이다.

## 🦧 Mulitpeer Connectivity의 구성요소

Multipeer Connectivity는 주로 다음 네 가지 주요 클래스에서 구성된다.

- **MCSession**: 장치 간의 연결을 관리하며, 데이터를 송수신하는 역할을 한다.
- **MCPeerID**: 장치의 고유 ID로, 장치 간의 구분을 위한 식별자 역할을 한다.
- **MCNearbyServiceAdvertiser**: 다른 장치들에게 자신의 존재를 알리는 역할을 한다.
- **MCNearbyServiceBrowser**: 근처에서 자신을 광고하고 있는 장치들을 검색하는 역할을 한다.

간단히 말해 각 장치는 자신을 광고하거나 다른 장치에 연결 요청을 보내서 네트워크에 참여할 수 있다. 장치 간의 연결이 확립되면, 데이터를 실시간으로 송수신할 수 있다. 송수신할 수 있는 데이터 유형에는 **텍스트, 이미지, 파일**, 그리고 **커스텀 데이터**까지 포함된다. 다만, 송수신하는 데이터는 반드시 **`Data` 형식**으로 변환되어야 하며, 이 데이터를 통해 앱은 장치 간의 통신을 처리한다.

### 🦍 MCNearbyServiceAdvertiser? MCAdvertiserAssistant?

![MCNearbyServiceAdvertiser](https://github.com/user-attachments/assets/eb5a9d2b-7a8a-43a4-bf2f-6494271e9621)

## 🦧 **Lumos!**

**Multipeer Connectivity**를 사용해 연결된 장치의 플래시를 조작해보도록 하겠다. 연결된 장치에 메시지를 보내고, 해당 장치에서 받은 메시지에 따라 플래시를 켜거나 끄는 기능을 구현하면 된다.

### 🦍 Setup

**Multipeer Connectivity**를 사용하기 위해서는 `info.plist` 에 2가지를 추가해줘야 한다.

1. Privacy - Local Network Usage Description
2. Bonjour services
    1. item 0 : `_flashlight._udp` 
    2. item 1 : `_flashlight._tcp`

flashlight는 임의로 지정한 것으로 각자 설정할 **서비스타입**과 동일하게 작성해주면 된다.<br>
ex) _(서비스타입)._udp / _(서비스타입)._tcp

### 🦍 MultipeerManager

```swift
// 내 피어 ID
var myPeerID = MCPeerID(displayName: UIDevice.current.name)

// 서비스 타입
let serviceType = "flashlight"

// 세션, 광고자, 브라우저 초기화
private var session: MCSession!
private var advertiser: MCNearbyServiceAdvertiser!
private var browser: MCNearbyServiceBrowser!
```

```swift
// 광고 시작
func startAdvertising() {
    advertiser.startAdvertisingPeer()
    isAdvertising = true
}

// 광고 중지
func stopAdvertising() {
    advertiser.stopAdvertisingPeer()
    isAdvertising = false
}
```

```swift
// 브라우징 시작
func startBrowsing() {
    browser.startBrowsingForPeers()
    isBrowsing = true
}

// 브라우징 중지
func stopBrowsing() {
    browser.stopBrowsingForPeers()
    isBrowsing = false
}
```

```swift
// 방에 참여
func joinRoom(peerID: MCPeerID) {
    browser.invitePeer(peerID, to: session, withContext: nil, timeout: 30)
}

// 연결 끊기
func disconnect() {
    // 모든 연결된 피어와의 연결 끊기
    session.disconnect()
    
    // 연결된 피어 목록을 비우기
    connectedPeers.removeAll()
}
```

### 🦍 FlashlightManager

```swift
// 비디오 장치 중 기본 장치 가져오기
private var device: AVCaptureDevice? {
    return AVCaptureDevice.default(for: .video)
}
```

### 🦍 데이터 송수신

```swift
// MultipeerManager.swift

extension MultipeerManager: MCSessionDelegate, MCNearbyServiceAdvertiserDelegate, MCNearbyServiceBrowserDelegate {
    // 명령 처리
    private func handleReceivedCommand(data: Data) {
        guard let command = data.first else { return }
        let isFlashlightOn = command == 1
        flashlightManager.toggleFlashlight(isOn: isFlashlightOn)
    }
    
    // 데이터 수신
    func session(_ session: MCSession, didReceive data: Data, fromPeer peerID: MCPeerID) {
        DispatchQueue.main.async {
            self.handleReceivedCommand(data: data)
        }
    }
}
```

```swift
// FlashlightManager.swift

func toggleFlashlight(isOn: Bool) {
    guard let device = device, device.hasTorch else { return }
    do {
        try device.lockForConfiguration()
        device.torchMode = isOn ? .on : .off
        device.unlockForConfiguration()
    } catch {
        print("Error toggling flashlight: \(error)")
    }
}
```

### 🦍 결과
