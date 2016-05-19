title: Processing symbol files 경고(?)
date: 2016-05-19 20:53:37
category:
- dev
- Xcode
tags:
- error
- iOS
- Xcode
---
Swift로 iOS 앱 개발 공부 중 아이폰을 직접 연결하여 실제 단말기에 실행 시키려 하니 아래와 같은 메세지가 뜨면서 실행이 중지되었다.

![Processing symbol files](/assets/images/processing-symbol-files.png)

<!-- more -->

구글링을 해보니 iOS를 업데이트하고 처음 Xcode에 연결하면 나타나는 증상으로 해당 디바이스로부터 디버깅을 위한 symbol files(구체적으로 어떤 역할을 하는지는 잘 모름)을 Xcode에서 인덱싱을 마치면 정상적으로 디바이스에 실행 및 테스트할 수 있게 된다.

필자는 iOS 9.3.1 --> iOS 9.3.2 로 업데이트하고 위와 같은 증상이 나타났다. 조금 기다리고 프로세싱이 끝난후 실행하니 정상적으로 실행되었다.
