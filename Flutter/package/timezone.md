## 플러터에서 timezone을 고려하는 방법
프로젝트를 진행하다가 해외 유저를 고려하여 개발을 해야하는 상황이 생겼다.
하지만 기존의 API는 한국시간의 날짜 데이터만 전해주는 상황이었다.
그러면 해외에서 앱을 사용하는 유저입장에서는 
미래에서 작성한 글이 보이는 경우도 생길 것이다.

그래서 내가 사용한 timezone을 고려하여 날짜를 계산하는 방법을 공유하고자 한다.

나는 **timezone**이라는 패키지를 사용했다.
> https://pub.dev/packages/timezone

1. main.dart에 timezone 데이터를 초기화
```dart
import 'package:timezone/data/latest.dart' as tz;
import 'package:timezone/timezone.dart' as tz;

void main() async {
  ...
  tz.initializeTimeZones();
}
```

2. timezone을 다루는 DateTimeUtil 작성
```dart
import 'package:timezone/timezone.dart';

class DateTimeUtil {
  // 기본 타임존을 Asia/Seoul로 지정
  static String timezone = 'Asia/Seoul';
  
  // 현지 시간을 한국시간으로 변환할때 사용
  static final korTimeZone = getLocation('Asia/Seoul');
}
```

3. main.dart에서 현지시간으로 초기화
```dart
import 'package:timezone/data/latest.dart' as tz;
import 'package:timezone/timezone.dart' as tz;

void main() async {
  ...
  tz.initializeTimeZones();
  var locations = tz.timeZoneDatabase.locations;

  for (var element in locations.entries) {
    if (element.value.currentTimeZone.abbreviation ==
        DateTime.now().timeZoneName) {
      debugPrint(element.value.name);
      debugPrint(element.value.currentTimeZone.abbreviation);
      DateTimeUtil.timezone = element.value.name;
      break;
    }
  }
}
```
Datetime.now의 timeZoneName을 가져오면 **KST**로 뜨는데
내가 필요한건 Asia/Seoul이기 때문에
timezone 데이터에서 불러온 location 정보를 하나하나 비교하여
DateTimeUtil.timezone에 값을 넣어주었다.
(더 효율적인 방법이 있으면 알려주세요)

4. 현지시간으로 변경
extension을 이용해서 현지시간으로 변경하는 함수를 작성하였다.
```dart
class DateTimeUtil {
  ...
}

extension DateTimeExtension on DateTime {
  DateTime get localTime {
    // 3에서 구한 timezone으로 location정보를 불러온다.
    final location = getLocation(DateTimeUtil.timezone);
    final localTimeZone =
        TZDateTime(location, year, month, day, hour, minute, second);
        
    // 한국시간이 +9이므로 -9를 해준다음
    final utc = add(const Duration(hours: -9));
    // local timezone의 offset만큼 더해주었다.
    final localTime = utc.add(localTimeZone.timeZoneOffset);
    
    return localTime;
  }
}
```
그러면 이제 
```dart
Text(
  news.createTime.localTime.toString(),
),
```
같이 사용하면 사용자의 timezone에 맞는 시간으로 표시된다.


### 현지시간을 한국시간으로 변경하는 방법
반대로 DateTime.now()를 했을 때 나오는 현지시간을
한국시간으로 변경하는 방법
```dart
// DateTime.now
tz.TZDateTime.now(DateTimeUtil.korTimeZone);

// 특정 날짜를 변경할 경우
DateTime(2024, 1, 7).add(
  Duration(
    milliseconds: DateTimeUtil.korTimeZone.currentTimeZone.offset,
  ),
);
```
