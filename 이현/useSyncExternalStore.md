## useSyncExternalStore

### 요약

외부 데이터 저장소에서 값을 읽는다.
외부 데이터 저장소는 React의 상태관리 시스템과 동기화 되어 있지 않은 모든 저장소를 말한다.
js 변수도 여기에 포함될 수 있다.

이 훅은 2~3개의 인자를 받는다.

`useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)`

- subscribe 함수 - store를 구독하는 로직이 존재해야 한다. store 구독을 취소하는 로직을 반환해야한다.

- getSnapshot 함수 - 현재 외부 저장소에 존재하는 데이터를 반환해야 한다.
  - getSnapshot을 통해 가져온 외부 데이터를 스냅샷으로 활용해 렌더링한다.
- getServerSnapshot 함수 - 서버 사이드 렌더링에 사용된다.

컴포넌트 내부에서 이 훅을 사용할 일은 거의 없다.
일반적으로 커스텀 훅 내부에서 사용한다.

**❗️ 주의 사항**

- getSnapshot이 반환하는 데이터는 읽기전용이어야 한다.

  직접 수정하지 말고, store에서 제공되는 메서드로 수정해야 한다.

  만약 반환된 데이터를 직접 수정한다면 React는 Object.is로 변화를 감지하기 때문에 리렌더링을 발생시키지 않을 것이다.

- 불필요하게 subscribe 함수를 컴포넌트 내에 정의하지말고, 컴포넌트 외부로 빼라

  컴포넌트 내부에 subscribe 함수를 정의하면, 컴포넌트가 렌더링 될 때마다 react가 다시 구독을 실시해야만 한다.

### 주요 용도

외부 데이터를 React의 State처럼 변화가 생길 때마다 리렌더링 시키고 싶을 때 사용한다.

특히 브라우저에서 가져오는 값을 이용해 그 값이 변할 때마다 화면을 다르게 띄워야 한다면 이 훅이 매우 유용할 것 같다.

### 예시

```tsx
function subscribe(callback) {
  window.addEventListener("online", callback);
  window.addEventListener("offline", callback);
  return () => {
    window.removeEventListener("online", callback);
    window.removeEventListener("offline", callback);
  };
}
```

React가 만들어 넣어주는 callback(listener)를 브라우저의 online/offline 이벤트 리스너로 등록해준다

### 질문 모음

**Q1. 구독한다는 말이 무엇인가?**

외부 저장소의 데이터가 변할 때, 실행되는 함수를 등록한다는 것이다.
이 함수를 listener라고 했을 때, listener 함수는 React가 자체적으로 만들어주는 함수이다.

그리고 React는 이를 useSyncExternalStore의 subscribe(첫 번째 파라미터)의 인자로 넣어준다.

**Q2. listener는 무슨 역할을 할까?**

Object.is 메서드를 이용해 외부 저장소의 이전 스냅샷 데이터와 현재 데이터를 비교한다.

그리고 그 값이 다르면 훅을 사용하는 컴포넌트를 리렌더링시킨다.

**Q3. listener를 subscribe로 받아서 등록은 했는데 어디에서 쓰이는 것인가?**

외부 저장소의 데이터를 수정할 때마다 React가 전달해준 listener를 실행시켜야 한다.

### 더 공부해야 할 키워드

1. 동시성 렌더링
2. 서버 사이드 렌더링
3. transition (non-blocking transition 업데이트)
