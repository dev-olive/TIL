---
id: part-1-overview-concepts
title: 'Redux Essentials, Part 1: Redux Overview and Concepts'
sidebar_label: 'Redux Overview and Concepts'
description: 'The official Essentials tutorial for Redux: learn how to use Redux, the right way'
---

import { DetailedExplanation } from '../../components/DetailedExplanation'

:::tip What You'll Learn

- Redux가 무엇이며 왜 사용하고 싶은지
- Redxu의 주요 용어와 개념
- Redux app을 통해 데이터가 어떻게 흐르는지

:::

## Introduction

Redux 핵심 튜토리얼에 온 것을 환영합니다! **이 튜토리얼에서는 Redux를 소개하고 최신 권장 도구와 모범 사례들을 통해 올바른 방법으로 Redux를 사용하는 방법을 알려줍니다.** 튜토리얼이 끝날 때 즈음, 여기에서 배운 도구와 패턴을 활용하여 자신만의 Redux 애플리케이션을 만들 수 있을 것입니다. 

튜토리얼의 1부에서는 Redux를 사용하기 위해 알아야 할 주요 개념과 용어를 다루고, [2부: Redux 앱 구조](./part-2-app-structure.md)에서는 기본 React + Redux 앱을 조사하여 각각의 부분들이 서로 잘 맞는지 보겠습니다.

[3부: 기본 Redux 데이터 흐름](./part-3-data-flow.md)에 들어가면서, 해당 지식을 사용하여 몇가지 실제 기능이 있는 작은 소셜 미디어 피드 앱을 구축하고, 이러한 기능들이 실제로 어떻게 작동하는지 확인하고, Redux 사용을 위한 몇가지 중요한 패턴과 가이드라인에 대해 설명할 것입니다. 

### How to Read This Tutorial

이 페이지는 Redux를 올바른 방법으로 사용하는 방법에 중점을 두고, Redux 앱을 올바르게 구축하는 방법을 이해할 수 있도록 개념을 충분히 설명합니다.

이러한 설명들을 초보자에게 친숙하게 유지하려고 노력했지만, 이미 알고 있는 것에 대해 몇가지 가정을 해야합니다. 

:::important 전제조건

- [HTML & CSS](https://internetingishard.com/)에 익숙
- [ES6 구문 및 기능](https://www.taniarascia.com/es6-syntax-and-feature-overview/)에 익숙
- React 용어에 대한 지식: [JSX](https://reactjs.org/docs/introducing-jsx.html), [State](https://reactjs.org/docs/state-and-lifecycle.html), [Function Components, Props](https://reactjs.org/docs/components-and-props.html),[Hooks](https://reactjs.org/docs/hooks-intro.html)
- [비동기 JavaScript](https://javascript.info/promise-basics) [AJAX 요청](https://javascript.info/fetch)에 대한 지식

:::

**해당 주제에 아직 익숙하지 않다면 먼저 시간을 내 익숙해진 다음에 다시 돌아와 Redux에 대해 알아보는 것이 좋다고 권장하고 싶습니다.** 당신이 준비되면 우리는 여기 있을 것입니다!

브라우저에 React 및 Redux Devtools 확장이 설치되었는지 확인해야 합니다.

- React DevTools Extension:
  - [React DevTools Extension for Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
  - [React DevTools Extension for Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)
- Redux DevTools Extension:
  - [Redux DevTools Extension for Chrome](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)
  - [Redux DevTools Extension for Firefox](https://addons.mozilla.org/en-US/firefox/addon/reduxdevtools/)

## What is Redux?

우선 "Redux"가 무엇인지 이해하는데 도움이 됩니다. 무엇을 할까요? 어떤 문제를 해결하는데 도움이 되나요? 왜 사용하고 싶나요?

**Redux는 "액션"이라는 이벤트를 사용하여 애플리케이션의 상태를 관리하고 업데이트하기 위한 패턴 및 라이브러리입니다.** 이는 전체 애플리케이션에 사용해야 하는 상태에 대한 중앙 저장소 역할을 합니다. (상태가 예측 가능한 방식으로만 업데이트 될 수 있다는 규칙이 있습니다.)

### Why Should I Use Redux?

Redux는 "전역"상태(애플리케이션의 많은 부분에 필요함)를 관리하는데 도움을 줍니다. 

**Redux에서 제공하는 패턴과 도구를 사용하면 애플리케이션의 상태가 언제 어디서 왜 어떻게 업데이트 되는지 쉽게 이해할 수 있습니다. 또한, 이러한 변화가 발생할 때 애플리케이션 로직이 어떻게 작동하는지 더 쉽게 이해할 수 있습니다.** Redux는 예측 가능하고 테스트 가능한 코드를 작성하게 하여, 애플리케이션이 예상대로 작동할 것이라는 확신을 줍니다. 

### When Should I Use Redux?

Redux는 공유 상태 관리를 처리하는데 도움을 주지만, 다른 도구와 마찬가지로 장단점이 있습니다. 배워야 할 개념과 작성해야 할 코드가 더 많습니다. 또한 코드에 간접 참조를 추가하고 특정 제한사항을 따르도록 요청합니다. 이는 단기 생산성과 장기 생산성 사이의 균형입니다. 

Redux는 이럴 때 더 유용합니다. 

- 앱 안의 여러 곳에서 많은 양의 애플리케이션의 상태가 있을 때
- 앱 상태가 시간이 지남에 따라 자주 업데이트 될 때
- 상태를 업데이트 하는 논리가 복잡할 때
- 앱이 중형/대형 코드베이스가 있고 여러 사람에 의 작업 될 때

**Not all apps need Redux. Take some time to think about the kind of app you're building, and decide what tools would be best to help solve the problems you're working on.**

모든 앱이 Redux가 필요한 것은 아닙니다. 시간을 내서 어떤 앱을 만들고 있는지 생각해보고 작업중인 문제를 해결하는데 가장 도움이 되는 도구를 결정하세요.

:::info 더 알고 싶다면?

Redux가 앱에 적합한지 확신이 들지 않다면, 아래 리소스가 추가 안내를 제공합니다:

- **[Redux에 도달할 때와 그렇지 않을 때](https://changelog.com/posts/when-and-when-not-to-reach-for-redux)**
- **[The Tao of Redux, Part 1 - 구현 및 의도](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)**
- **[Redux FAQ: 언제 Redux를 사용해야 할까?](../../faq/General.md#when-should-i-use-redux)**
- **[Redux가 필요하지 않을 수 있습니다.](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)**

:::

### Redux Libraries and Tools

Redux는 작은 독립형 JS 라이브러리입니다. 하지만 일반적으로 여러 다른 패키지와 함께 사용됩니다. 

#### React-Redux

Redux는 모든 UI 프레임워크와 통합할 수 있으며 React에서 가장 자주 사용됩니다. React-Redux는 우리의 공식적인 패키지이다. [**React-Redux**](https://react-redux.js.org/)는 상태 조각을 읽고 저장소에 업데이트 하기 위한 액션을 전달하여, React 구성요소들이 Redux 저장소와 상호작용 할 수 있게 하는 공식패키지입니다.

#### Redux Toolkit

[**Redux Toolkit**](https://redux-toolkit.js.org)은 Redux 로직을 작성할 때 가장 권장되는 접근 방식입니다.  여기에는 Redux 앱을 구축할 때 필수적이라고 생각되는 패키지와 기능이 포함되어 있습니다. Redux Toolkit은 제안된 모범사례들을 기반으로 하여, 대부분의 Redux 작업을 단순화 하고 일반적인 실수들을 방지하며, Redux 애플리케이션을 더 쉽게 작성할 수 있게 합니다.

#### Redux DevTools Extension

[**Redux DevTools Extension**](https://github.com/reduxjs/redux-devtools/tree/main/extension)는 시간 경과에 따른 Redux 저장소의 상태 변경 내역을 보여줍니다. 이는 "time-travel 디버깅"과 같은 강력한 기술을 사용하는 것을 포함하여 애플리케이션을 효과적으로 디버그할 수 있게 해줍니다. 

## Redux Terms and Concepts

실제 코드에 들어가기 앞서, Redux를 사용하기 위해 알아야 할 몇몇 용어와 개념에 대해 알아보겠습니다. 

### State Management

Let's start by looking at a small React counter component. It tracks a number in component state, and increments the number when a button is clicked:

작은 React 카운터 구성요소부터 살펴보겠습니다. 구성 요소 상태의 숫자를 추적하고, 버튼이 클릭되면 숫자가 증가합니다.

```jsx
function Counter() {
  // State : 카운터 value
  const [counter, setCounter] = useState(0)

  // Action : 이벤트 발생 시 상태 업데이트를 하는 코드
  const increment = () => {
    setCounter(prevCounter => prevCounter + 1)
  }

  // View: UI 정의
  return (
    <div>
      Value: {counter} <button onClick={increment}>Increment</button>
    </div>
  )
}
```

다음 부분으로 구성된 독립형 앱입니다.

- **상태(state)** , 앱을 구동하는 진실의 소스
- **뷰(view)**, 현재 상태에 기반한 UI의 선언적 설명
- **액션(action)**, 앱에서 사용자 입력을 기반으로 발생 및 상태 업데이트를 유발하는 이벤트

**"단방향 데이터 흐름"**의 작은 예시

- 상태는 특정 시점의 앱의 상태를 나타낸다.
- UI는 해당 상태에 기반해 렌더링 된다.
- 사용자가 버튼을 클릭하는 것 등과 같은 일이 발생하면 상태는 발생한 것에 따라 업데이트된다.
- UI는 새로운 상태에 기반해 리렌더링된다.

![One-way data flow](/img/tutorials/essentials/one-way-data-flow.png)

**하지만 같은 상태를 공유하고 사용해야 하는 여러 구성요소가 있는 경우**, 특히 이러한 구성요소가 앱의 다른 부분에 있는 경우 단순성은 무너질 수 있습니다. 때때로 이것은 상위 구성요소로 "상태를 끌어올림(lifting state up)"으로써 해결할 수 있지만 항상 도움이 되는 것은 아닙니다.

이를 해결하기 위한 한가지 방법은 구성요소에서 공유된 상태를 추출하고 이를 구성요소 트리 외부의 중앙 위치에 두는 것입니다. 이를 통해 구성 요소 트리는 큰 "vie"가 되고 모든 구성 요소들은 트리의  위치와 상관없이 상태에 접근할 수 있거나 액션을 야기할 수 있습니다!

상태관리와 관련된 개념을 정의하고 분리하며, 보기와 상태 사이의 독립성을 유지하는 규칙을 적용함으로써 코드에 더 많은 구조와 유지 관리성을 제공합니다.

이것이 Redux의 기본 아이디어입니다 : 애플리케이션 안에 전역 상태를 포함하는 단일 중앙 위치와 코드를 예측 가능하게 만들기 위해 해당 상태가 업데이트 될 때 따라야 하는 특정 패턴입니다. 

### Immutability

"Mutable"은 "변경 가능"을 의미합니다. 어떤 것이 "immutable"이면, 절대 변경할 수 없습니다.

JavaScript 객체와 배열은 기본적으로 모두 변경 가능합니다(mutable). 객체를 생성하면 해당 필드의 내용을 변경할 수 있습니다. 배열을 생성하면 마찬가지로 그 내용을 변경할 수 있습니다.

```js
const obj = { a: 1, b: 2 }
// 계속 같은 객체이지만 내용은 변경되었습니다.
obj.b = 3

const arr = ['a', 'b']
// 같은 방법으로 이 배열의 내용을 변경할 수 있습니다.
arr.push('c')
arr[1] = 'd'
```

이것을 객체 또는 배열의 *변형*이라고 합니다. 메모리에 있는 동일한 객체(또는 배열) 참조이지만, 객체 안의 내용은 변경되었습니다.

**값을 변경할 수 없도록 업데이트 하려면, 코드에서 반드시 기존 객체/배열에서 사본을 만든 후 그 사본을 수정해야 합니다.**

Javascript의 배열/객체 확산 연산자를 활용하여 이 작업을 직접 수행할 수 있습니다. 또한 원래 배열을 변경하는 대신 배열의 새로운 복사본을 반환하는 배열 메서드를 사용하여 이 작업을 직접 수행할 수 있습니다. 

```js
const obj = {
  a: {
    // obj.a.c를 안정하게 업데이트하기 위해 각각을 복사해야 합니다.
    c: 3
  },
  b: 2
}

const obj2 = {
  // copy obj
  ...obj,
  // overwrite a
  a: {
    // copy obj.a
    ...obj.a,
    // overwrite c
    c: 42
  }
}

const arr = ['a', 'b']
// arr의 복사본을 생성하고 "c"를 뒤에 붙입니다. 
const arr2 = arr.concat('c')

// 기존 배열의 사본을 만들 수도 있습니다. 
const arr3 = arr.slice()
// 그리고 사본을 변경합니다.
arr3.push('c')
```

**Redux expects that all state updates are done immutably**. We'll look at where and how this is important a bit later, as well as some easier ways to write immutable update logic.

**Redux는 모든 상태의 업데이트가 불변으로 수행되기를 예상합니다.** 이것이 중요한 위치와, 불변 업데이트 논리를 더 쉽가 작성하는 방법을 조금 후에 알아보겠습니다.

:::info 더 알고 싶다면?

JavaScript에서 불변성이 작동하는 방식에 대해 더 알고 싶다면:

- [JavaScript의 참조에 대한 시각적 가이드](https://daveceddia.com/javascript-references/)
- [React와 Redux에서의 불변성 : 완전한 가이드](https://daveceddia.com/react-redux-immutability-guide/)

:::

### Terminology

계속하기 앞서 숙지해야 할 몇가지 중요한 Redux 용어가 있습니다.

#### Actions

**액션**은 일반 JavaScript 객체로 `type` 필드를 가집니다. **액션을 애플리케이션에서 발생한 일을 설명하는 이벤트라고 생각할 수 있습니다.** 

`type`필드는 액션을 설명하는 이름을 제공하는 문자열이어야 합니다.(예: `"todos/todoAdded"`) 보통 `"domain/eventName"`과 같이 type 문자열을 작성합니다. 첫번째 부분은 이 액션이 속한 기능이나 범주이고, 두번째 부분은 발생한 특정한 일입니다.

액션 객체는 발생한 것에 대한 다른 추가적인 정보가 있는 필드를 가질 수 있습니다. 관례에 따라, 그런 정보를 `payload`라는 필드에 넣습니다.

일반적인 액션 객체는 다음과 같습니다.

```js
const addTodoAction = {
    type: 'todos/todoAdded',
    payload : 'Buy milk'
}
```

#### Action Creators

**액션 생성자**는 액션 객체를 생성하고 반환하는 함수입니다. 일반적으로 이것을 사용하므로 매번 손으로 액션 객체를 작성하지 않아도 됩니다.

```js
const addTodo = text => {
    return {
        type: 'todos/todoAdded',
        payload: text
    }
}
```

#### Reducers

리듀서는 현재 `상태(state)`와 `액션(action)`객체를 받고 필요한 경우 상태를 어떻게 업데이트 할지 결정하고, 새 상태를 반환하는 함수입니다 : `(state, action) => newState`. **리듀서는 받은 액션(이벤트) 유형에 따라 이벤트를 처리하는 이벤트 리스너라고 생각할 수 있습니다.** 

:::info

"리듀서"함수는 `Array.reduce()`메소드에 전달하는 일종의 콜백함수와 비슷하기 때문에 그 이름을 얻습니다.

:::

리듀서는 *항상* 몇가지 특정 규칙을 따라야 합니다.

- `state`와 `action`인수를 기반으로 새로운 상태값만 계산해야 합니다. 

- 기존의 `state`는 수정할 수 없습니다. 대신 기존의 `state`를 복사하고 복사된 값을 변경하여 *불변 업데이트*를 수행해야 합니다. 
- 비동기 논리를 수행하거나, 임의의 값을 계산하거나, 기타 "부작용"을 야기해서는 안됩니다. 

왜 중요한지, 어떻게 올바르게 따라야 하는지를 포함하여 리듀서의 규칙들에 대해서 추후 더 자세히 이야기할 것입니다.

리듀서 함수 안의 로직 일반적으로 동일한 일련의 단계를 따릅니다:

- 리듀서가 이 액션에 관련이 있는지 확인하세요.
  - 만약 그렇다면, 상태 복사본을 만들고, 복사본을 새로운 값으로 업데이트하고 반환하세요.
  
- 그렇지 않다면, 기존 상태를 변경하지 않고 반환합니다.

다음은 각 리듀서가 따라야 하는 단계를 보여주는 리듀서의 간단한 예제입니다.

```js
const initialState = { value: 0 }
function counterReducer(state = initialState, action) {
	// 리듀서가 이 액션에 관련있는지 확인하세요
    if(action.type === 'counter/increment') {
        // 만약 그렇다면 `상태`의 목사본을 만들고
        return {
            ...state,
            //복사본을 새로운 값으로 업데이트하세요
            value: state.value + 1
        }
    }
    // 그렇지 않다면 기존의 상태를 변경하지 않고 반환하세요
    return state
}
```

Reducers can use any kind of logic inside to decide what the new state should be: `if/else`, `switch`, loops, and so on.

리듀서는 

<DetailedExplanation title="Detailed Explanation: Why Are They Called 'Reducers?'" >

The [`Array.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) method lets you take an array of values, process each item in the array one at a time, and return a single final result. You can think of it as "reducing the array down to one value".

`Array.reduce()` takes a callback function as an argument, which will be called one time for each item in the array. It takes two arguments:

- `previousResult`, the value that your callback returned last time
- `currentItem`, the current item in the array

The first time that the callback runs, there isn't a `previousResult` available, so we need to also pass in an initial value that will be used as the first `previousResult`.

If we wanted to add together an array of numbers to find out what the total is, we could write a reduce callback that looks like this:

```js
const numbers = [2, 5, 8]

const addNumbers = (previousResult, currentItem) => {
  console.log({ previousResult, currentItem })
  return previousResult + currentItem
}

const initialValue = 0

const total = numbers.reduce(addNumbers, initialValue)
// {previousResult: 0, currentItem: 2}
// {previousResult: 2, currentItem: 5}
// {previousResult: 7, currentItem: 8}

console.log(total)
// 15
```

Notice that this `addNumbers` "reduce callback" function doesn't need to keep track of anything itself. It takes the `previousResult` and `currentItem` arguments, does something with them, and returns a new result value.

**A Redux reducer function is exactly the same idea as this "reduce callback" function!** It takes a "previous result" (the `state`), and the "current item" (the `action` object), decides a new state value based on those arguments, and returns that new state.

If we were to create an array of Redux actions, call `reduce()`, and pass in a reducer function, we'd get a final result the same way:

```js
const actions = [
  { type: 'counter/increment' },
  { type: 'counter/increment' },
  { type: 'counter/increment' }
]

const initialState = { value: 0 }

const finalResult = actions.reduce(counterReducer, initialState)
console.log(finalResult)
// {value: 3}
```

We can say that **Redux reducers reduce a set of actions (over time) into a single state**. The difference is that with `Array.reduce()` it happens all at once, and with Redux, it happens over the lifetime of your running app.

</DetailedExplanation>

#### Store

The current Redux application state lives in an object called the **store** .

The store is created by passing in a reducer, and has a method called `getState` that returns the current state value:

```js
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

#### Dispatch

The Redux store has a method called `dispatch`. **The only way to update the state is to call `store.dispatch()` and pass in an action object**. The store will run its reducer function and save the new state value inside, and we can call `getState()` to retrieve the updated value:

```js
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}
```

**You can think of dispatching actions as "triggering an event"** in the application. Something happened, and we want the store to know about it. Reducers act like event listeners, and when they hear an action they are interested in, they update the state in response.

We typically call action creators to dispatch the right action:

```js
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState())
// {value: 2}
```

#### Selectors

**Selectors** are functions that know how to extract specific pieces of information from a store state value. As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data:

```js
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

### Redux Application Data Flow

Earlier, we talked about "one-way data flow", which describes this sequence of steps to update the app:

- State describes the condition of the app at a specific point in time
- The UI is rendered based on that state
- When something happens (such as a user clicking a button), the state is updated based on what occurred
- The UI re-renders based on the new state

For Redux specifically, we can break these steps into more detail:

- Initial setup:
  - A Redux store is created using a root reducer function
  - The store calls the root reducer once, and saves the return value as its initial `state`
  - When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render. They also subscribe to any future store updates so they can know if the state has changed.
- Updates:
  - Something happens in the app, such as a user clicking a button
  - The app code dispatches an action to the Redux store, like `dispatch({type: 'counter/increment'})`
  - The store runs the reducer function again with the previous `state` and the current `action`, and saves the return value as the new `state`
  - The store notifies all parts of the UI that are subscribed that the store has been updated
  - Each UI component that needs data from the store checks to see if the parts of the state they need have changed.
  - Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen

Here's what that data flow looks like visually:

![Redux data flow diagram](/img/tutorials/essentials/ReduxDataFlowDiagram.gif)

## What You've Learned

Redux does have a number of new terms and concepts to remember. As a reminder, here's what we just covered:

:::tip Summary

- **Redux is a library for managing global application state**
  - Redux is typically used with the React-Redux library for integrating Redux and React together
  - Redux Toolkit is the recommended way to write Redux logic
- **Redux uses a "one-way data flow" app structure**
  - State describes the condition of the app at a point in time, and UI renders based on that state
  - When something happens in the app:
    - The UI dispatches an action
    - The store runs the reducers, and the state is updated based on what occurred
    - The store notifies the UI that the state has changed
  - The UI re-renders based on the new state
- **Redux uses several types of code**
  - _Actions_ are plain objects with a `type` field, and describe "what happened" in the app
  - _Reducers_ are functions that calculate a new state value based on previous state + an action
  - A Redux _store_ runs the root reducer whenever an action is _dispatched_

:::

## What's Next?

We've seen each of the individual pieces of a Redux app. Next, continue on to [Part 2: Redux App Structure](./part-2-app-structure.md), where we'll look at a full working example to see how the pieces fit together.