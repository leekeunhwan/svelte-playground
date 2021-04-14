# Svelte Study

---

프로젝트 생성 하는 방법 : `npx degit sveltejs/template ${MY_PROJECT}`,

<br/>

## Svelte 특징

---

`$ (reative)`

이벤트에 대한 응답과 같이 DOM을 애플리케이션 상태와 동기화 상태로 유지하기 위한 강력한 반응성 시스템

-> 리액트의 경우 state에 \* 2를 해둔 것을 useMemo를 통해 두거나 혹은 JSX 부분에서 계산을 해두는 편인데
-> 이에 비해 좀 더 간소화 되었다는 느낌도 받고, 단지 변수에 그치는 것이 아니라 로직이나 조건문에도
-> reactive하게 반응하는 것을 보면서 vue3에 비해서도 좋은 느낌을 받았다.

```js
// 다음과 같이 할당한다
<script>
	let count = 0;
	$: doubled = count * 2;

	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

// count가 1이면,  doubled가 2, count가 2이면, doubled가 4가 된다.
<p>{count} doubled is {doubled}</p>



// 이렇게 조건을 할당할 수도 있고, 단일 로직, Block (코드의 집합)도 리액티브하게 관리할 수 있다.
<script>
	let count = 0;

	$: if (count >= 10) {
		alert(`count is dangerously high!`);
		count = 9;
	}

	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

<br/>

`stateless`

React와 Vue와 다르게, Svelte는 그냥 블록안에 변수를 두고, 변수에 값을 재할당하는 식으로 관리해주면 된다.<br/>
배열의 경우 push, splice를 하면 바로 갱신되지 않고, 반드시 변수에 원하는 값을 만들어서 재할당 하는 식으로 관리해야한다.<br/>

```js
// 이러면 갱신이 안됨 (X)
function addNumber() {
  numbers.push(number.length + 1);
}

// 반드시 이렇게 해줘야 갱신이 된다. (O)
function addNumber() {
  numbers = [...numbers, numbers.length + 1];
}

// 객체의 경우는 react와 유사하다.

// 이 경우는 동일 레퍼런스이기 때문에 값이 같이 변한다.
const obj = {
  foo: "foo",
};

const test = obj;
test.a = 123;

console.log(obj); // Object { foo: "foo", a: 123 }
consol.elog(obj); // Object { foo: "foo", a: 123 }

// 얕은 복사를 통해서 immutable하게 관리해줘야 한다.
const obj = {
  foo: "foo",
};

const test = { ...obj };
test.a = 123;

console.log(obj); // Object { foo: "foo" }
consol.elog(obj); // Object { foo: "foo", a: 123 }
```

<br/>

`props`

개념은 React와 Vue와 동일하다.<br/>
부모가 자식에게 값을 넘겨줄 때 사용한다.<br/>
여기서도 무분별하게 중첩된 Props를 운영하면<br/>
Props Drilling과 같은 부분에서 어려움이 있을 수 있기에<br/>
상태관리를 할 때 어디서 어디까지 Props Drilling을 할 것인지 정해야한다.<br/>
<br/>

사용하는 방법은 React와 유사한데<br/>
부모로부터 Props를 받아서 처리하는 방법이 다르다.<br/>
<br/>

동일한 이름의 변수를 선언하되, export라는 키워드를 붙여주어야<br/>
props로 넘겨준 값을 받아서 사용할 수 있다.<br/>
보통 대게 할당없이 변수를 선언하는 것 같은 모양인데<br/>
여기에 할당을 해주면 defaultValue로 작동하게 된다.<br/>

Rest Parameter를 이용해서 한번에 풀어서 넘길 수도 있고,<br/>
객체로 넘겨서 자식에서 받아서 원하는 것을 구조분해할당하여 분리하여 사용할 수 있다.<br/>

```js
// Parent
<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42}/>


// Child
<script>
    export let answer;
</script>

<p>The answer is {answer}</p>



// use DefaultPropValue

<script>
    export let answer = 100;
</script>

<p>The answer is {answer}</p>
```

<br/>

`If Block`

Handlebars와 유사한 모양의 문법이다.<br/>
`#`을 통해 블록을 열고, `:`을 통해 블록을 연결하고, `/`을 통해 블록을 닫는다.<br/>

```js
// user.loggedIn이 true일때 해당 버튼은 보이게 된다.
// 다소 생소하긴 하지만, Vue의 v-if보다 더 명확하고, react의 컨디셔널 렌더보다 잘 읽힌다.
{#if user.loggedIn}
    <button on:click={logout}>Log out</button>
{/if}

// vue의 v-if와 같다.
<button v-if="user.loggedIn" on:click="logout">Log out</button>

// react의 컨디셔널 렌더랑 같다.
{user.loggedIn && <button on:click={logout}>Log out</button>}


// if-else
{#if user.loggedIn}
    <button on:click={logout}>Log out</button>
    {:else}
    <button on:click={login}>Log In</button>
{/if}


// if-elseif-else
{#if x > 100}
    <p>세자리 수</p>
    {:else if x > 10}
    <p>두자리 수</p>
    {:else}
    <p>한자리 수</p>
{/if}
```

<br/>

`Each Block`

데이터 목록을 반복해야하는 경우 각 블록을 사용하는 것<br/>
쉽게 말해 배열을 리스트로 표현할 때 해당 블록을 사용하면 된다.<br/>
IF 블록과 마찬가지로 `#`으로 열고, `/`으로 닫는다.<br/>
key는 `{#each cats as cat (cat.id)}` 이런식으로 마지막에 괄호를 열어 넣어준다.<br/>

```js
//
<ul>
	{#each cats as cat (cat.id)}
		<li><a target="_blank" href="https://www.youtube.com/watch?v={cat.id}">
			{cat.name}
		</a></li>
	{/each}
</ul>
```

<br/>

`Await Block`

IF 블록과 마찬가지로 `#`으로 열고, `:`로 연결하고, `/`으로 닫는다.<br/>

```js
// in React
const {isLoading, error, data: userData} = userInfoQuery();

if (isLoading) {
  return <LoadingIndicator />;
}

if (error) {
  return <ErrorRedirectScreen />;
}

return <UserProfile userData={userData} />;


// in Svelte
{#await promise}
    <LoadingIndicator/>
{:then userData}
    <UserProfile userData={userData} />
{:catch error}
    <ErrorRedirectScreen />
{/await}
```

<br/>

`DOM 이벤트`

이건 Vue와 비슷하다.<br/>
Vue의 `v-on:`과 같이 Svelte는 `on:`을 붙여주면 된다.<br/>

<br/>

```js
<script>
  let m = { x: 0, y: 0 };

  function handleMousemove(event) {
    m.x = event.clientX;
    m.y = event.clientY;
  }
</script>

<style>
  div {
    width: 100%;
    height: 100%;
  }
</style>

<div on:mousemove={handleMousemove}>The mouse position is {m.x} x {m.y}</div>

// 혹은

<div on:mousemove={(event) => m = {x: event.clientX, y: event.clientY}}>
  The mouse position is {m.x} x {m.y}
</div>
```

여기까지는 유사하지만, 이런 부분에서 편의성이 증대되었다고 느껴졌다.<br/>

| modifier 이름     | 동작 내용                                                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `preventDefault`  | event.preventDefault()와 동일, 태그의 기본 동작 방지                                                                        |
| `stopPropagation` | event.stopPropagation()과 동일, 이벤트 전파 방지                                                                            |
| `passive`         | 터치 혹은 휠 이벤트로 발생하는 스크롤 성능을 향상시키는 이벤트 수식어 (안전하다고 판단되면 svelte가 알아서 적용시켜 줍니다) |
| `nonpassive`      | passive 효과를 쓰지 않을 때 사용 (= {passive: false})                                                                       |
| `capture`         | 버블링 단계가 아닌 캡쳐링 단계에서 이벤트 핸들러를 실행합니다.                                                              |
| `once`            | 이벤트 핸들러를 단 한번만 실행하도록 하는 이벤트 수식어                                                                     |
| `self`            | `event.target`과 이벤트 핸들러를 정의한 요소가 같을 때 이벤트 핸들러를 실행하도록 하는 이벤트 수식어                        |

<br/>

`passive는 어떻게 스크롤 성능을 향상시키는걸까?`

```
브라우저에서는 터치 혹은 휠 이벤트가 발생할 때 마다 스크롤을 할지 말지 결정해야 합니다.
스크롤이 발생하면 많은 이벤트가 발생하게 되는데, 이벤트 마다 스크롤을 할지 말지 판단하게 된다면 브라우저는 느려질 수 있습니다.


passive 속성을 사용하면, 스크롤을 막지 않겠다고 브라우저에게 알려주게 되어,
브라우저는 항상 스크롤을 하는 것으로 인식하고, 스크롤 할지 말지 결정하는데 낭비되는 자원을 절약해서 성능을 향상 시킬 수 있게 됩니다.

addEventListener의 passive 옵션과 동일한 효과입니다.
```

```html
<script>
  function handleClick() {
    alert("clicked");
  }
</script>

<!-- 이런식으로 속성값들을 체이닝해서 사용할 수 있습니다. -->
<button on:click|once|preventDefault="{handleClick}">Click me</button>
```

<br/>

`createEventDispatcher`

외부 컴포넌트에서 정의한 함수를 내부에서 호출할 때는 해당 메소드를 통해<br/>
dispatch를 생성하고, dispatch를 통해 해당 함수에 해당하는 이벤트 이름을 호출해야한다.<br/>

```js
// Parent

<script>
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Inner on:message={handleMessage}/>

// Child

<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>



// like React => 이렇게도 동작은 합니다.
// 하지만 가급적 위처럼 쓰는게 좋습니다.
// Parent
<script>
	import Inner from './Inner.svelte';

	function handleMessage(text) {
		alert(text);
	}
</script>

<Inner handleMessage={handleMessage}/>

// Child
<script>
export let handleMessage;

	function sayHello() {
		handleMessage('hello')
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

<br/>

`Event Forwarding`

DOM 이벤트와 다르게 구성 요소 이벤트는 버블링 되지 않습니다.<br/>
깊이 중첩된 일부 구성 요소에서 이벤트를 수신하려면 중간 구성 요소가 이벤트를 전달해야합니다.<br/>

```js
// Parent
<script>
	import Outer from './Outer.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Outer on:message={handleMessage}/>

// Middle
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message/>

// Child
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

<br/>

`DOM Event Forwarding`

부모로부터 on:click 이벤트를 받은 상태에서 자식이 on:click을 하고<br/>
호출할 함수가 없다면 부모의 이벤트를 호출하게 됩니다.<br/>

```js
// Parent
<script>
	import CustomButton from './CustomButton.svelte';

	function handleClick() {
		console.log('123')
	}
</script>

<CustomButton on:click={handleClick}/>

// Child
<style>
	button {
		height: 4rem;
		width: 8rem;
		background-color: #aaa;
		border-color: #f1c40f;
		color: #f1c40f;
		font-size: 1.25rem;
		background-image: linear-gradient(45deg, #f1c40f 50%, transparent 50%);
		background-position: 100%;
		background-size: 400%;
		transition: background 300ms ease-in-out;
	}
	button:hover {
		background-position: 0;
		color: #aaa;
	}
</style>


// When Button Click => '123'
<button on:click>
	Click me
</button>

```

<br/>

`Binding`

일반적으로 Svelte의 데이터 흐름은 하향식입니다.<br/>
부모 구성 요소는 자식 구성 요소에 소품을 설정할 수 있고<br/>
구성 요소는 요소에 속성을 설정할 수 있지만 그 반대는 설정할 수 없습니다.<br/>

하지만 때때로 그 규칙을 어기는 것이 유용합니다.<br/>
`bind:`을 이용하면 됩니다.<br/>

IF, Each 블록등.. 안에서도 사용가능합니다.<br/>
attribute에 붙는 것은 거의 바인딩이 가능합니다.<br/>

```js
// svelte
<script>
  let name = "world";
</script>

<input bind:value={name} />

<h1>Hello {name}!</h1>
```

```js
// Numeric Input도 bind:value로 처리해주면 됩니다.
<script>
  let a = 1;
  let b = 2;
</script>

<label>
  <input type="number" bind:value={a} min="0" max="10" />
  <input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
  <input type="number" bind:value={b} min="0" max="10" />
  <input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>
```

```js
// Checkbox Input도 이렇게 써주면 됩니다.'
<input type="checkbox" bind:checked={yes} />
```

```js
// Group Inputs은 bind:group을 이용하여 처리한다.

<script>
	let scoops = 1;
	let flavours = ['Mint choc chip'];

	let menu = [
		'Cookies and cream',
		'Mint choc chip',
		'Raspberry ripple'
	];

	function join(flavours) {
		if (flavours.length === 1) return flavours[0];
		return `${flavours.slice(0, -1).join(', ')} and ${flavours[flavours.length - 1]}`;
	}
</script>

<h2>Size</h2>

<label>
	<input type="radio" bind:group={scoops} value={1}>
	One scoop
</label>

<label>
	<input type="radio" bind:group={scoops} value={2}>
	Two scoops
</label>

<label>
	<input type="radio" bind:group={scoops} value={3}>
	Three scoops
</label>

<h2>Flavours</h2>

{#each menu as flavour}
<label>
<input type="checkbox" bind:group={flavours} value={flavour}>
{flavour}
</label>
{/each}

{#if flavours.length === 0}

<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
<p>Can't order more flavours than scoops!</p>
{:else}
<p>
You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
of {join(flavours)}
</p>
{/if}
```

```js
// textarea의 경우에도 bind:value를 통해 처리할 수 있습니다.
<script>
	let value = `Some words are *italic*, some are **bold**`;
</script>

<textarea bind:value></textarea>
<p>{value}</p>
```

```js
// select의 경우도 비슷한데 조금 다릅니다.
// 여러개를 고르도록 하고 싶을 때는 mulitple 어트리뷰트를 추가해줍니다.

<script>
	let questions = [
		{ id: 1, text: `Where did you go to school?` },
		{ id: 2, text: `What is your mother's name?` },
		{ id: 3, text: `What is another personal fact that an attacker could easily find with Google?` }
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
	}
</script>

<style>
	input { display: block; width: 500px; max-width: 100%; }
</style>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
	<select bind:value={selected} on:change="{() => answer = ''}">
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>

	<input bind:value={answer}>

	<button disabled={!answer} type=submit>
		Submit
	</button>
</form>

<p>selected question {selected ? selected.id : '[waiting...]'}</p>
```

```js
// contenteditable을 true로 설정하면 bind:innerHTML 혹은 bind:textContent를 지원해줍니다.

<script>
	let html = '<p>Write some text!</p>';
</script>

<div
	contenteditable="true"
	bind:textContent={html}
></div>

<pre>{html}</pre>

<style>
	[contenteditable] {
		padding: 0.5em;
		border: 1px solid #eee;
		border-radius: 4px;
	}
</style>
```

<br/>

## 스벨트에 대한 생각

```
React랑 비교해봤을 때,
html에 조건과 같은 코드블록을 제공함으로서,
기존처럼 메소드에 조건이 붙지 않게되고,
액션에 더욱 집중할 수 있도록 환경을 제공해주는 것을 느낄 수 있었다.

스크립트는 스크립트에 집중하고,
스타일은 스타일에 집중하고
html은 html에 집중하는 Vue의 SFC와도 상당부분 닮아 있는 이부분은
한 흐름에 컴포넌트 파악이 용이하고, 각각 역할 수행에 최선을 다함으로
React에서 JSX, CSS-IN-JS와 같이 경계가 모호해지는 것을 방지해준다.

단순히 Svelte를 Vue의 Sugar Syntax Tool이라고 생각했는데,
이는 잘못된 생각인 것 같다. 상당히 잘 짜여져 있고,
충분히 어플리케이션에 무리가 없는 툴인 것 같다.
```
