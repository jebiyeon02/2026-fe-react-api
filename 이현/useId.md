## useId

### 키워드

접근성, 서버 사이드 렌더링

### 요약

`aria-describedb`같은 접근성 어트리뷰트에 전달할 수 있는 고유 ID를 생성하기 위한 훅

- 매개변수 X
- useId를 리스트의 key를 생성하기 위해 사용하면 안 된다.

html에서 쓰는 것처럼 id를 React 컴포넌트 내에서 직접 지정할 수 있음. 하지만 그러면 컴포넌트가 렌더링 될 때마다 새로운 id 인스턴스가 만들어지는 것이기 때문에 완전히 고유하지 않음.

useId로 생성된 id는 렌더링 사이에서도 React가 그 고유성을 보장해줄 수 있음.

### 주요 용도

접근성을 위해 서로 다른 태그를 연결하기 위함

### 예시

```tsx

<>
   <input type="password" aria-describedby={passwordHintId} />
   <p id={passwordHintId}>
</>

```
