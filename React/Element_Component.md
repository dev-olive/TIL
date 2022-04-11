## ELEMENT / COMPONENT

### element

React element는 불변객체.

엘리먼트를 생성한 이후에는 해당 엘리먼트의 자식이나 속성을 변경할 수 없다. 엘리먼트는 영화에서 하나의 프레임과 같이 특정 시점의 UI를 보여준다.

UI를 업데이트하는 유일한 방법은 새로운 엘리먼트를 생성하고 이를 `ReactDOM.render()`로 전달하는 것

```react
function tick(){
  const element = (
    <div>
      <h1>Hello, world</h1>
      <h2> It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  )
  ReactDOM.render(element, document.getElementById('root'));
} 

setInterval(tick, 1000);
```

`setInterval()`콜백을 이용해 초마다 `ReactDOM.render()`를 호출한다. (실제로 대부분의 React앱은 `ReactDOM.render()`)를 한번만 호출함

React DOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 DOM을 원하는 상태로 만드는데 필요한 경우에만 DOM을 업데이트함.

매초 전체의 UI를 다시 그리도록 엘리먼트를 만들었지만 React DOM은 내용이 변경된 텍스트 노드만 업데이트한다.

### components

```react
function Welcome(props){
  return <h1>Hello, {props.name}</h1>;
}
const element = <Welcome name="Sara"/>;

ReactDOM.render(
  element,
  document.getElementById('root'),
)
```

1. `<Welcome name="Sara" />` 엘리먼트로 `ReactDOM.render()`를 호출한다.
2. React는 `{name : 'Sara'}`를 props로 하여 `Welcome`컴포넌트를 호출한다.
3. `Welcome`컴포넌트는 결과적으로 `<h1>Hello, Sara</h1>` 엘리먼트를 반환한다.
4. ReactDOM은 `<h1>Hello Sara</h1>` 엘리먼트와 일치하도록 



React는 매우 유연하지만 한 가지 엄격한 규칙이 있다.

**모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 한다.**
