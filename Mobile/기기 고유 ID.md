# 기기 고유 ID를 얻는 방법
회원가입 없이 사용자를 구분하기 위해  
기기 고유 ID가 필요했다.

### 안드로이드
[공식문서](https://developer.android.com/training/articles/user-data-ids?hl=ko#common-use-cases)  
안드로이드의 경우에는 권장사항으로 
- 앱 재설치 시 변경되어도 상관 없는 경우 인스턴스 ID 또는 GUID를 사용
- 앱 재설치에도 유지되어야 하는 경우 SSAID 사용

### IOS
예전에는 UDID를 사용했지만  
현재는 UDID를 사용하는 경우 앱심사를 통과할 수 없다.  
**대안으로 자체 UUID를 생성한 다음 key chain에 저장**하여  
앱 재설치 시에도 값을 유지하는 방법이 있다.
