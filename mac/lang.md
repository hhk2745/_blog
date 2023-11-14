# 한영


/Library/LaunchAgents/com.local.KeyRemapping.plist 또는
~/Library/LaunchAgents/com.local.KeyRemapping.plist


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.local.KeyRemapping</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/hidutil</string>
        <string>property</string>
        <string>--set</string>
        <string>{"UserKeyMapping":[
            {
              "HIDKeyboardModifierMappingSrc": 0x7000000E7,
              "HIDKeyboardModifierMappingDst": 0x70000006D
            },
            {
              "HIDKeyboardModifierMappingSrc": 0x7000000E6,
              "HIDKeyboardModifierMappingDst": 0xFF00000003
            }
        ]}</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

이후 계정 로그아웃 또는 재부팅을 진행해줍니다.

시스템 환경설정
시스템 환경설정 > 키보드 > 단축키 > 입력 소스
입력 메뉴에서 다음 소스 선택에 오른쪽 command 키를 눌렀을 때 F18이 입력되는지 확인합니다.

![v](https://user-images.githubusercontent.com/18409941/282364569-51e92421-c5a7-4b13-b270-a39371b3740f.png "입력 소스 shortcut 설정")
_입력 소스 shortcut 설정_
