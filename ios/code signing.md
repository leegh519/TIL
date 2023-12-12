# code signing

ios 앱은 모두 code signing이라는 절차를 거치게 된다.  
애플에서 발급받은 인증서를 통해 서명을 하는 절차로  
믿을 수 있는 앱을 보장하는 애플의 정책이다.  

보통은 Xcode에서 Automatically manage signing을 사용해서  
어떻게 돌아가는지 인지하지 못하고 있었는데 fastlane을 적용하다 보니  
이 과정을 직접 설정해줘야해서 찾아보게 되었다.  

Apple Developer에서  
- Apple Development
- Apple Distribution
2가지 인증서를 발급받을 수 있다.  


<https://sujinnaljin.medium.com/ios-certificate-%EC%99%80-provisioning-profile-e1b9455e8a51>
