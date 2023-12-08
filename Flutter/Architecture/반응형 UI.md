# 반응형 UI

### 목차

1. [레이어](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%EB%A0%88%EC%9D%B4%EC%96%B4.md#%EB%A0%88%EC%9D%B4%EC%96%B4)
2. [반응형 UI](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%EB%B0%98%EC%9D%91%ED%98%95%20%EC%82%AC%EC%9A%A9%EC%9E%90%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4.md#반응형-UI)
3. [위젯](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%EC%9C%84%EC%A0%AF.md#위젯)
4. [렌더링 프로세스](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%EB%A0%8C%EB%8D%94%EB%A7%81%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4.md#렌더링-프로세스)
5. [플랫폼 임베더](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%ED%94%8C%EB%9E%AB%ED%8F%BC%20%EC%9E%84%EB%B2%A0%EB%8D%94.md#플랫폼-임베더)
6. [다른 코드와 통합](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%EB%8B%A4%EB%A5%B8%20%EC%BD%94%EB%93%9C%EC%99%80%20%ED%86%B5%ED%95%A9.md#다른-코드와-통합)
7. [플러터 웹](https://github.com/leegh519/TIL/blob/main/Flutter/Architecture/%ED%94%8C%EB%9F%AC%ED%84%B0%20%EC%9B%B9.md#플러터-웹)


## 반응형 UI
플러터는 반응형, 선언형 UI 프레임워크이다.   
여기서 말하는 반응형은 웹 디자인에서 말하는 반응형과는 조금 다른 개념으로   
어플리케이션의 상태변화에 따라 UI의 상태를 업데이트 시킨다는 개념으로 이해할 수 있다.   

선언형 UI란 개발자가 UI의 요소와 상태를 선언하고    
상태에 따라 UI를 업데이트하는 방식을 말한다.
이와 반대되는 개념은 명령형 UI이다.   

예를 들어 살펴보면    
명령형 UI는
```javascript
b.setColor(red)
b.clearChildren()
ViewC c3 = new ViewC(...)
b.add(c3)
```
개발자가 UI의 모든 요소를 직접 컨트롤한다면   

선언형 UI는
```javascript
return ViewB(
  color: red,
  child: const ViewC(),
);
```
UI를 선언한 다음 색깔이나 자식요소를 변경하려면    
개발자는 해당 상태만 변경시키면 UI를 업데이트 할 수 있는 방식이다.   

> ℹ️ **참고:**    
> 안드로이드의 xml을 명령형 UI의 예시로 드는 경우가 있는데   
> 엄밀히 말하자면 xml은 선언형으로 UI를 구성하지만   
> UI의 상태를 변경하는 부분에서 명령형 UI의 방식을 사용한다고 볼 수 있다.   
> jetpack compose는 온전히 선언형 UI의 방식을 사용한다.   
   
<br/>   

플러터에서 UI를 구성하는 단위는 위젯이다.   
위젯은 build() 메서드를 통해 UI를 생성한다.   
이 메서드는 상태를 UI로 변환한다.   
```
UI = f(state)
```
<br/>   

build 메서드는 빠르게 동작해야하며, side effect가 없어야 한다.    
dart 언어의 빠른 객체 생성과 삭제라는 특징은    
플러터 프레임워크의 동작에 적합하다.   

