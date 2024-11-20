# SVELTE-WEB-BROWSER

## 반응성

```js
let count = $state(0);
```

- 스벨트는 앱 state와 DOM을 연동하기 위한 반응성 시스템을 갖추고 있음
- state는 재할당에 반응, mutations에도 반응하는데 이를 deep reactivity라고 함
- deep reactivity은 proxy를 통해 구현됨

## if문

```js
{#if count > 10}
    <p>{count} is greater than 10</p>
{/if}
```

## Promise

```js
let promise = $state(roll());
```

- 프로미스를 state로 정의
- 위의 코드에서 roll()이 프로미스를 반환

```js
{#await promise}
    <p>...rolling</p>
{:then number}
    <p>you rolled a {number}!</p>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}
```

- UI단은 위와 같이 구현

## Bindings

```js
<input bind:value={name}>
```

- 양방향 데이터 바인딩이 가능
- v-model과 동일

### Group bindings

```js
<input
    type="radio"
    name="scoops"
    value={number}
    bind:group={scoops}
/>
```

- 위와 같은 형태로 그룹 바인딩이 가능
- scoops에는 선택된 모든 값이 담김
- radio에서는 하나의 값만, checkbox와 같이 여러 개 선택 가능한 경우에는 여러 개 값

### class bindings

```js
<button
    class="card"
    class:flipped={flipped}
    onclick={() => flipped = !flipped}
></button>
```

- class 바인딩을 위와 같이 심플하게 처리 가능
- `class:fliped` 로도 적용 가능

## css 변수

```js
<style>
    .box {
        width: 5em;
        height: 5em;
        border-radius: 0.5em;
        margin: 0 0 1em 0;
        background-color: var(--color, #ddd);
    }
</style>
<div class="boxes">
    <Box --color="red" />
    <Box --color="green" />
    <Box --color="blue" />
</div>
```

- css 변수를 위와 같이 동적으로 처리 가능

## use directive

```js
<div class="menu" use:trapFocus>
```

- vue 커스텀 디렉티브와 동일
- 디렉티브 내에 effect를 정의해서 마운트, 언마운트시 동작을 구현할 수 있음

## Svelte Kit

- Svelte가 컴포넌트 프레임워크라면 Svelte Kit은 배포에 적합한 앱을 만들기 위한 앱 프레임워크
- Svelte Kit은 파일 시스템 기반 라우팅을 사용
- 앱의 경로는 코드베이스의 디렉토리에 의해 정의됨
- 가령 /about이라는 경로의 페이지를 만들 경우
  - routes 폴더 하위에 about 폴더 생성
  - +page.svelte 파일 생성
  - about으로 라우팅시 +page.svelte에 정의된 내용이 나옴
- 동적 라우팅을 하고 싶다면 폴더명을 [slug]로 만들고 그 하위에 페이지를 정의
- SvelteKit의 핵심 역할은 총 3가지
  - 라우팅
  - 로딩 - 라우트에 필요한 데이터 가져오기
  - 렌더링
- 앱의 모든 페이지에서 load 함수를 정의 가능
  - 특정 페이지 하나만 있을 경우 +page.server.js에서 load를 정의
  - 하위 페이지들이 있을 경우 최상위 페이지에서 +layout.server.js에 load를 정의할 수도 있음
- 각 페이지에서 $props를 통해 데이터 접근 가능
- 공유해서 사용되는 모듈들은 src/lib 폴더에 정의