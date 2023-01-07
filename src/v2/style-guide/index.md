---
title: Style Guide
type: style-guide
---

# 스타일 가이드

이 문서는 Vue 코드에 대한 공식 스타일 가이드입니다. 만약 현재 Vue를 사용하여 프로젝트를 진행 중이라면 이 문서는 에러와 바이크쉐딩 (bikeshedding), 안티패턴을 피하는 좋은 참조가 될 것 입니다. 그러나 이 문서에서 제시하는 스타일 가이드가 당신의 프로젝트에 무조건 적합한 것은 아닙니다. 그러므로 당신의 경험과 기술 스택, 개인적 통찰력을 바탕으로 이 스타일 가이드가 적용되는 것을 권장해드립니다.

대부분의 경우 우리는 HTML과 자바스크립트에 대한 제안은 일반적으로 피합니다. 우리는 당신이 세미콜론이나 쉼표(trailing commas)에 대한 사용 여부는 신경 쓰지 않습니다. 우리는 당신이 HTML의 속성값을 위해 작은따옴표를 사용하는지 큰따옴표를 사용하는지 신경 쓰지 않습니다. 그러나 특정 패턴이 뷰 컨텍스트에서 유용하다고 발견된 경우 예외가 존재합니다.

> **곧, 우린 실행에 필요한 팁들도 제공할 예정입니다.** 단순한 수준의 사전지식이 필요한 경우도 있지만(have to be disciplined), 가급적 ESLint를 비롯한 다른 자동화된 프로세스들을 더 쉽게 실행할 수 있도록 예시를 들어 설명하고 있습니다.

마지막으로, 우리는 규칙을 4가지 범주로 분류하였습니다.


## 규칙 분류

### 우선순위 A: 필수

이 규칙은 오류를 예방하는데 도움을 주기 때문에 모든 비용을 통해서 학습하고 준수하여야 합니다. 예외 상황이 존재하겠지만 매우 드물며 자바스크립트와 뷰에 대한 전문 지식이 있어야 만들 수 있습니다.

### 우선순위 B: 매우 추천함

이 규칙은 대부분의 프로젝트에서 가독성 그리고 개발자 경험을 향상시키는 것으로 발견되었습니다. 해당 규칙을 위반해도 코드는 여전히 실행되지만 위반은 드물고 정당합니다.

### 우선순위 C: 추천함

동일하게 좋은 여러 가지 옵션이 존재하는 경우, 일관성을 보장하기 위해 임의의 선택을 할 수 있습니다. 이 규칙은 각각의 수용 가능한 옵션을 설명하고 기본 선택을 제안합니다. 즉, 일관성 있고 좋은 이유가 있으면 당신의 코드베이스에서 자유롭게 다른 선택을 할 수 있습니다. 좋은 이유가 있어야 합니다! 커뮤니티 표준에 적응되기 위해서 당신은 다음과 같이 해야 합니다.

1. 당신이 마주하는 대부분의 커뮤니티 코드를 더 쉽게 분석할 수 있도록 훈련하세요
2. 커뮤니티 코드 예제를 수정하기 한고 복사 그리고 붙혀넣기 할 수 있어야 합니다
3. 적어도 뷰에 있어서는 당신이 선호하는 코딩 스타일을 수용하는 새로운 직원을 자주 찾을 것입니다

### 우선순위 D: 주의 요함

뷰의 일부 특성은 드문 엣지 케이스 또는 레거시 코드로의부터 마이그레이션을 위해 존재합니다. 그러나 그것들을 남용하면 당신의 코드를 유지 보수하기 어렵게 만들거나 버그를 발생시키는 원인이 될 수 있습니다. 이 규칙은 잠재적 위험요소를 인식시켜주고 언제 그리고 왜 피해야 하는지 설명해 줍니다.



## 우선순위 A 규칙: 필수 (에러 방지)



### 컴포넌트 이름에 합성어 사용 <sup data-p="a">필수</sup>

**root 컴포넌트인 `App` 과 `<transition>`, `<component>`등 Vue에서 제공되는 빌트인 컴포넌트를 제외하고 컴포넌트의 이름은 항상 합성어를 사용해야 한다.**

모든 HTML 엘리먼트의 이름은 한 단어이기 때문에 합성어를 사용하는 것은 기존 그리고 향후 HTML 엘리먼트와의 [충돌을 방지해줍니다](http://w3c.github.io/webcomponents/spec/custom/#valid-custom-element-name).

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

Vue.component('todo', {
  // ...
})
```

``` js
export default {
  name: 'Todo',
  // ...
}
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
Vue.component('todo-item', {
  // ...
})
```

``` js
export default {
  name: 'TodoItem',
  // ...
}
```
{% raw %}</div>{% endraw %}



### 컴포넌트 데이터 <sup data-p="a">필수</sup>

**컴포넌트의 `data` 는 반드시 함수여야 합니다.**

컴포넌트(i.e. `new Vue`를 제외한 모든 곳)의 `data` 프로퍼티의 값은 반드시 객체(object)를 반환하는 함수여야 한다.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

`data` 의 값이 오브젝트일 경우, 컴포넌트의 모든 인스턴스가 공유한다. 예를 들어, 다음 data 를 가진 `TodoList` 컴포넌트를 상상해보자.

``` js
data: {
  listTitle: '',
  todos: []
}
```

이 컴포넌트는 재사용하여 사용자가 여러 목록(e.g. 쇼핑, 소원, 오늘 할 일 등)을 유지할 수 있도록 해야 할 수 있다. 컴포넌트의 모든 인스턴스가 동일한 data 객체를 참조하므로, 하나의 목록의 타이틀을 변경할 때 다른 모든 리스트의 타이틀도 변경될 것이다. Todo를 추가/수정/삭제하는 경우에도 마찬가지다.

대신 우리는 각 컴포넌트의 인스턴스 자체 data만을 관리하기를 원한다. 이렇게 하려면 각 인스턴스는 고유한 data 객체를 생성해야 한다. JavaScript에서는 함수안에서 객체를 반환하는 방법으로 해결할 수 있다:

``` js
data: function () {
  return {
    listTitle: '',
    todos: []
  }
}
```
{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
Vue.component('some-comp', {
  data: {
    foo: 'bar'
  }
})
```

``` js
export default {
  data: {
    foo: 'bar'
  }
}
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음
``` js
Vue.component('some-comp', {
  data: function () {
    return {
      foo: 'bar'
    }
  }
})
```

``` js
// In a .vue file
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```

``` js
// It's OK to use an object directly in a root
// Vue instance, since only a single instance
// will ever exist.
new Vue({
  data: {
    foo: 'bar'
  }
})
```
{% raw %}</div>{% endraw %}



### Props 정의 <sup data-p="a">필수</sup>

**Prop은 가능한 상세하게 정의되어야 합니다.**

커밋 된 코드에서, prop 정의는 적어도 타입은 명시되도록 가능한 상세하게 정의되어야 합니다.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

자세한 [prop definitions](https://vuejs.org/v2/guide/components.html#Prop-Validation) 두 가지 이점을 갖는다:

- 이 API는 컴포넌트의 API를 문서화하므로 컴포넌트의 사용 방법을 쉽게 알 수 있다.
- 개발 중에, Vue는 컴포넌트의 타입이 잘못 지정된 props를 전달하면 경고 메시지를 표시하여 오류의 잠재적 원인을 파악할 수 있도록 도와준다.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
// This is only OK when prototyping
props: ['status']
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
props: {
  status: String
}
```

``` js
// Even better!
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
  }
}
```
{% raw %}</div>{% endraw %}



### `v-for` 에 `key` 지정  <sup data-p="a">필수</sup>

**`v-for`는 `key`와 항상 함께 사용합니다.**

서브트리의 내부 컴포넌트 상태를 유지하기 위해 `v-for`는 _항상_ `key`와 함께 요구됩니다. 비록 엘리먼트이긴 하지만 에니메이션의 [객체 불변성](https://bost.ocks.org/mike/constancy/)과 같이 예측 가능한 행동을 유지하는 것은 좋은 습관입니다.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

할 일 목록이 있다고 가정 해보자:

``` js
data: function () {
  return {
    todos: [
      {
        id: 1,
        text: 'Learn to use v-for'
      },
      {
        id: 2,
        text: 'Learn to use key'
      }
    ]
  }
}
```

그다음 알파벳순으로 정렬한다. DOM 이 업데이트될 때, Vue는 가능한 적은 DOM 전이(mutations)를 수행하기 위해 렌더링을 최적화한다. 즉, 첫 번째 할 일 엘리먼트를 지우고, 리스트의 마지막에 다시 추가한다.

문제는, DOM에 남아있을 요소를 삭제하지 않는 것이 중요한 경우가 있다는 것입니다. 예를 들어, ``을 사용하여 list 정렬을 활성화하거나, 렌더링 된 요소가 ``인 경우 포커스를 유지하는 경우가 있습니다. 이러한 경우, 각 항목에 대해 고유한 키(예: `:key="todo.id"`)를 추가하면 Vue가 어떻게 행동하는지 더욱 예측이 가능할 것입니다.

경험에 비추어 볼 때, 당신과 당신의 팀이 이러한 edge case 들에 대해 걱정할 필요가 없도록 _항상_ unique key를 추가하는 것이 좋습니다. 따라서 개체의 항상성이 필요하지 않은 드문 performance-critical 시나리오에서는 의식적으로 예외를 적용할 수 있습니다.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```
{% raw %}</div>{% endraw %}



### `v-if`와 `v-for`를 동시에 사용하지 마세요 <sup data-p="a">필수</sup>

**`v-for`가 사용된 엘리먼트에 절대 `v-if`를 사용하지 마세요.**

일반적으로 다음과 같은 상황에서 이러한 오류를 범합니다:

- 리스트 목록을 필터링 하려는 경우(e.g. `v-for="user in users" v-if="user.isActive"`).  이 경우 users을 새로운 computed 속성으로 필터링된 목록으로 대체하십시오(e.g. `activeUsers`).

- 리스트의 일부 요소의 렌더링을 회피하려는 경우(e.g. `v-for="user in users" v-if="shouldShowUsers"`). 이 경우 `v-if`를 컨테이너 엘리먼트로 옮기세요 (e.g. `ul`, `ol`).

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

Vue는 디렉티브를 처리할 때, `v-for`를 `v-if`보다 높은 우선 순위로 처리합니다. 아래 내용에 따르면,

``` html
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

위 내용은 아래와 비슷합니다.

``` js
this.users.map(function (user) {
  if (user.isActive) {
    return user.name
  }
})
```
위와 같은 로직은 `users`중 `isActive`값을 기준으로 일부 유저만 렌더링 하려는 상황에서, 유저의 `isActive`상태 변화가 없더라도 `users`의 모든 요소를 다시 렌더링하는 문제가 있습니다.
( 역주 : `users`중 한 엘리먼트의 `isActive`값이 변하면, 그 변경사항을 화면에 반영하기 위해 `isActive`값의 변화가 없는 엘리먼트까지 전부 다시 렌더링하므로 무의미한 프로세싱이 발생함 )

아래와 같이 computed 속성을 활용하여 계산을 진행하면,
``` js
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}
```

``` html
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

다음과 같은 이점을 얻을 수 있습니다.

- 필터링된 리스트는 `users` 배열에 관련된 변경 사항이 있는 경우에만 다시 계산되므로, 필터링이 더 효율적입니다.
- `v-for="user in activeUsers"`를 활용하는 것은, 렌더링을 하는 동안 _active 상태의 유저만_ 계산하기에, 좀 더 효율적으로 렌더링합니다.
- 로직이 표현 계층과 분리되어, 유지 보수(변경/확장) 작업이 좀 더 쉬워집니다.

아래 내용처럼 업데이트를 통해 비슷한 효과를 볼 수 있습니다.
``` html
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

를 아래 내용으로 변경합니다.

``` html
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```


`v-if`를 컨테이너 요소로 이동함으로써, 더 이상 매번 리스트의 _모든 유저_에서 `shouldShowUsers`를 체크할 필요가 없어졌습니다. 대신에, 우리는 `shouldShowUsers`가 거짓일 때, `v-for`를 한번만 체크하거나 심지어 전혀 계산할 필요가 없습니다!
{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

``` html
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

``` html
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
{% raw %}</div>{% endraw %}



### 컴포넌트 스타일 스코프 <sup data-p="a">필수</sup>

**애플리케이션에 있어서, 최상위 `APP` 컴포넌트 및 레이아웃 컴포넌트의 스타일은 전역일 수 있으나, 다른 모든 컴포넌트는 범위가 지정되어야 합니다.**

이는 [싱글 파일 컴포넌트](../guide/single-file-components.html)에만 관련됩니다. [`scoped` 속성](https://vue-loader.vuejs.org/en/features/scoped-css.html)의 사용은 필수는 **아닙니다**. 스코프는 [CSS modules](https://vue-loader.vuejs.org/en/features/css-modules.html), [BEM](http://getbem.com/)과 같은 클래스 기반 전략, 혹은 다른 라이브러리/관례를 통해 이루어질 수 있습니다.

**그러나, 컴포넌트 라이브러리에서는 `scoped` 속성을 사용하는 대신에, 클래스 기반 전략이 오히려 바람직합니다.**

이것은 내부 스타일 덮어쓰기를 더 쉽게 만들어주며, 사람이 읽을 수 있는 클래스 이름이 매우 낮은 충돌 가능성과 함께 너무 높은 특이도를 갖지 않게 해 줍니다.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

대규모 프로젝트를 다른 개발자와 같이 개발하고 있거나, 혹은 Auth0같은 써드파티(3rd-party) HTML/CSS를 이용한다면, 범위 지정 스코프가 컴포넌트를 의도한대로 디자인 할 수 있도록 도와줍니다.

상기한 `scoped` 속성 외에도, 유일한 클래스 이름을 사용하는 것은 써드파티 CSS가 HTML에 원치 않게 적용되는 것을 방지할 수 있습니다. 예를 들어, 많은 프로젝트가 `button`, `btn`, 또는 `icon`같은 이름을 이미 사용하고 있습니다. 이를 이행하기 위해, BEM같은 방식을 사용하지 않더라도, 앱 또는 컴포넌트를 특정하는 접두사(e.g. `ButtonClose-icon`)을 사용하는 것이 바람직합니다.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<template>
  <button class="btn btn-close">X</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<template>
  <button class="button button-close">X</button>
</template>

<!-- Using the `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>
```

``` html
<template>
  <button :class="[$style.button, $style.buttonClose]">X</button>
</template>

<!-- Using CSS modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>
```

``` html
<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- Using the BEM convention -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```
{% raw %}</div>{% endraw %}



### Private 속성 이름 <sup data-p="a">필수</sup>

**플러그인, mixin 등에서 커스텀 사용자 private 프로터피에는 항상 접두사 `$_`를 사용하라. 그다음 다른 사람의 코드와 충돌을 피하려면 named scope를 포함하라. (e.g. `$_yourPluginName_`).**

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

Vue uses the `_` prefix to define its own private properties, so using the same prefix (e.g. `_update`) risks overwriting an instance property. Even if you check and Vue is not currently using a particular property name, there is no guarantee a conflict won't arise in a later version.

As for the `$` prefix, its purpose within the Vue ecosystem is special instance properties that are exposed to the user, so using it for _private_ properties would not be appropriate.

Instead, we recommend combining the two prefixes into `$_`, as a convention for user-defined private properties that guarantee no conflicts with Vue.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
var myGreatMixin = {
  // ...
  methods: {
    update: function () {
      // ...
    }
  }
}
```

``` js
var myGreatMixin = {
  // ...
  methods: {
    _update: function () {
      // ...
    }
  }
}
```

``` js
var myGreatMixin = {
  // ...
  methods: {
    $update: function () {
      // ...
    }
  }
}
```

``` js
var myGreatMixin = {
  // ...
  methods: {
    $_update: function () {
      // ...
    }
  }
}
```

{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
var myGreatMixin = {
  // ...
  methods: {
    $_myGreatMixin_update: function () {
      // ...
    }
  }
}
```

``` js
// Even better!
var myGreatMixin = {
  // ...
  methods: {
    publicMethod() {
      // ...
      myPrivateFunction()
    }
  }
}

function myPrivateFunction() {
  // ...
}

export default myGreatMixin
```
{% raw %}</div>{% endraw %}



## 우선순위 B 규칙: 매우 추천함 (가독성 향상을 위함)



### 컴포넌트 파일 <sup data-p="b">매우 추천함</sup>

**언제든지 빌드 시스템이 파일들을 하나로 합치는 것이 가능하기에, 컴포넌트마다 파일을 생성해서 관리해야합니다.**

이 과정은 당신이 수정하거나, 어떻게 사용하는 지 리뷰가 필요한 컴포넌트를 찾아낼 때 큰 도움이 됩니다.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
Vue.component('TodoList', {
  // ...
})

Vue.component('TodoItem', {
  // ...
})
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- TodoList.js
|- TodoItem.js
```

```
components/
|- TodoList.vue
|- TodoItem.vue
```
{% raw %}</div>{% endraw %}



### 싱글 파일 컴포넌트 이름 규칙 지정(casing) <sup data-p="b">매우 추천함</sup>

**[싱글 파일 컴포넌트(SFC)](../guide/single-file-components.html)의 파일명은 항상 PascalCase 또는 kebab-case 형태로 작명되어야 합니다.**

PascalCase는 templates과 JS, JSX 파일 내의 컴포넌트를 참조하는 방식과 거의 비슷하므로, 코드 편집기에서 자동 완성을 할 때 가장 효과적입니다.

그러나 여러 가지 형태로 섞여 있는 파일명은 대소문자에 민감한(case-sensitive) 시스템에서 문제를 일으킬 수 있습니다. 이럴 때는 kebab-case가 굉장히 효과적입니다.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- mycomponent.vue
```

```
components/
|- myComponent.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- MyComponent.vue
```

```
components/
|- my-component.vue
```
{% raw %}</div>{% endraw %}



### 베이스 컴포넌트 이름 <sup data-p="b">매우 추천함</sup>

**특정한 앱 스타일과 규약이 적용되는 (presentational, dumb, 혹은 pure components라 불리는) 베이스 컴포넌트는 `Base`, `App`, 또는 `V`와 같은 접두어로 이름이 시작되어야 합니다.**
{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

이러한 요소들은 당신의 어플리케이션에서 스타일링과 행위에 기초를 두고 있습니다. 해당 요소들은 **다음 항목만** 포함할 수 있습니다.

- HTML 요소,
- 다른 베이스 컴포넌트, 그리고
- 써드파티(3rd-Party) UI 컴포넌트.

그러나 이 요소들은 전역 상태(Vuex의 store)는 **절대** 포함하지 않습니다.

컴포넌트 이름에는 특정한 목적(e.g. `BaseIcon`)을 위한 요소가 있는 경우를 제외하고, `BaseButton`, `BaseTable` 같이 컴포넌트가 해당 요소를 감싸는 요소의 이름이 포함되는 경우가 많습니다. 만약 당신이 좀 더 구체적인 컨텍스트에 대해 비슷한 컴포넌트를 구성하는 경우, 컴포넌트들이 거의 항상 이 요소들을 소비하게 될 겁니다. 예를 들면, `BaseButton`이 `BaseSumbit`에서 쓰이듯이 말이죠.

이러한 표기 규약은 몇 가지 이점이 있습니다:

- 편집기에서 알파벳순으로 정렬할 때, 앱의 기본 요소들이 함께 정리되어, 좀 더 식별하기 쉽습니다.

- 컴포넌트 이름은 여러 단어로 구성되기 때문에, 이러한 규약은 간단한 구성요소 래퍼(e.g. `MyButton`, `VueButton`)를 위한 임의 접두사를 붙이는 것을 방지합니다.

- 이러한 요소들은 자주 쓰이기 때문에, 어디서든 import해 사용하는 것 대신, 전역적으로 선언하는 것이 권장됩니다. 접두사는 Webpack이 해당 작업을 수행할 수 있도록 합니다:

  ``` js
  var requireComponent = require.context("./src", true, /^Base[A-Z]/)
  requireComponent.keys().forEach(function (fileName) {
    var baseComponentConfig = requireComponent(fileName)
    baseComponentConfig = baseComponentConfig.default || baseComponentConfig
    var baseComponentName = baseComponentConfig.name || (
      fileName
        .replace(/^.+\//, '')
        .replace(/\.\w+$/, '')
    )
    Vue.component(baseComponentName, baseComponentConfig)
  })
  ```

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue
```

```
components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue
```

```
components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue
```
{% raw %}</div>{% endraw %}



### 싱글 인스턴스 컴포넌트 이름 <sup data-p="b">매우 추천함</sup>

**Components that should only ever have a single active instance should begin with the `The` prefix, to denote that there can be only one.**

This does not mean the component is only used in a single page, but it will only be used once _per page_. These components never accept any props, since they are specific to your app, not their context within your app. If you find the need to add props, it's a good indication that this is actually a reusable component that is only used once per page _for now_.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- Heading.vue
|- MySidebar.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- TheHeading.vue
|- TheSidebar.vue
```
{% raw %}</div>{% endraw %}



### 강한 연관성을 가진 컴포넌트 이름 <sup data-p="b">매우 추천함</sup>

**Child components that are tightly coupled with their parent should include the parent component name as a prefix.**

If a component only makes sense in the context of a single parent component, that relationship should be evident in its name. Since editors typically organize files alphabetically, this also keeps these related files next to each other.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

You might be tempted to solve this problem by nesting child components in directories named after their parent. For example:

```
components/
|- TodoList/
   |- Item/
      |- index.vue
      |- Button.vue
   |- index.vue
```

or:

```
components/
|- TodoList/
   |- Item/
      |- Button.vue
   |- Item.vue
|- TodoList.vue
```

This isn't recommended, as it results in:

- Many files with similar names, making rapid file switching in code editors more difficult.
- Many nested sub-directories, which increases the time it takes to browse components in an editor's sidebar.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
```

```
components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

```
components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```
{% raw %}</div>{% endraw %}



### 컴포넌트 이름의 단어 순서 정렬 <sup data-p="b">매우 추천함</sup>

**Component names should start with the highest-level (often most general) words and end with descriptive modifying words.**

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

You may be wondering:

> "Why would we force component names to use less natural language?"

In natural English, adjectives and other descriptors do typically appear before the nouns, while exceptions require connector words. For example:

- Coffee _with_ milk
- Soup _of the_ day
- Visitor _to the_ museum

You can definitely include these connector words in component names if you'd like, but the order is still important.

Also note that **what's considered "highest-level" will be contextual to your app**. For example, imagine an app with a search form. It may include components like this one:

```
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```

As you might notice, it's quite difficult to see which components are specific to the search. Now let's rename the components according to the rule:

```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputExcludeGlob.vue
|- SearchInputQuery.vue
|- SettingsCheckboxLaunchOnStartup.vue
|- SettingsCheckboxTerms.vue
```

Since editors typically organize files alphabetically, all the important relationships between components are now evident at a glance.

You might be tempted to solve this problem differently, nesting all the search components under a "search" directory, then all the settings components under a "settings" directory. We only recommend considering this approach in very large apps (e.g. 100+ components), for these reasons:

- It generally takes more time to navigate through nested sub-directories, than scrolling through a single `components` directory.
- Name conflicts (e.g. multiple `ButtonDelete.vue` components) make it more difficult to quickly navigate to a specific component in a code editor.
- Refactoring becomes more difficult, because find-and-replace often isn't sufficient to update relative references to a moved component.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```
{% raw %}</div>{% endraw %}



### 셀프 클로징 컴포넌트 <sup data-p="b">매우 추천함</sup>

**Components with no content should be self-closing in [single-file components](../guide/single-file-components.html), string templates, and [JSX](../guide/render-function.html#JSX) - but never in DOM templates.**

Components that self-close communicate that they not only have no content, but are **meant** to have no content. It's the difference between a blank page in a book and one labeled "This page intentionally left blank." Your code is also cleaner without the unnecessary closing tag.

Unfortunately, HTML doesn't allow custom elements to be self-closing - only [official "void" elements](https://www.w3.org/TR/html/syntax.html#void-elements). That's why the strategy is only possible when Vue's template compiler can reach the template before the DOM, then serve the DOM spec-compliant HTML.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<!-- In single-file components, string templates, and JSX -->
<MyComponent></MyComponent>
```

``` html
<!-- In DOM templates -->
<my-component/>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<!-- In single-file components, string templates, and JSX -->
<MyComponent/>
```

``` html
<!-- In DOM templates -->
<my-component></my-component>
```
{% raw %}</div>{% endraw %}



### 템플릿에서 컴포넌트 이름 규칙 지정(casing) <sup data-p="b">매우 추천함</sup>

**대부분의 프로젝트에서 [싱글 파일 컴포넌트(SFC)](../guide/single-file-components.html)와 문자열 템플릿의 컴포넌트 이름은 PascalCase 형태를 갖춰야합니다. DOM 템플릿의 컴포넌트 이름은 kebab-case 형태를 갖추면 됩니다.**

PascalCase는 kebab-case에 비해 몇 가지 이점을 지니고 있습니다 :
- 편집기가 템플릿에서 컴포넌트 이름을 자동완성 기능을 활용할 수 있습니다. 왜냐하면 PascalCase가 JavaScript에서도 쓰이기 때문입니다.
- `<MyComponent>`가 `<my-component>`보다 한 단어로 된 HTML 요소가 시각적으로 더 잘 구분됩니다. 두 문자(두 대문자)가 하나의 하이픈(-)보다 글자 차이가 많기 때문입니다.
- 만약 당신이 템플릿에서 웹 컴포넌트 같은 Vue의 요소가 아닌 커스텀 요소를 사용한다면, PascalCase가 당신의 Vue 컴포넌트를 시각적으로 잘 구분 될 수 있게 만들어줍니다.

불행하게도, HTML은 대소문자에 민감하지 않기 때문에(case-insensitive), DOM 템플릿은 kebab-case를 여전히 사용해야 합니다.
또한, 당신이 kebab-case를 이미 많이 사용하였거나, 투자를 많이 했을 때에는, HTML 표기의 일관성과 모든 프로젝트에 같은 규칙을 이행하는 것이 이점이 좀 더 있을 수 있습니다. **이런 경우는 kebab-case를 어디서든 사용하셔도 됩니다.**
{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<!-- In single-file components and string templates -->
<mycomponent/>
```

``` html
<!-- In single-file components and string templates -->
<myComponent/>
```

``` html
<!-- In DOM templates -->
<MyComponent></MyComponent>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<!-- In single-file components and string templates -->
<MyComponent/>
```

``` html
<!-- In DOM templates -->
<my-component></my-component>
```

OR

``` html
<!-- Everywhere -->
<my-component></my-component>
```
{% raw %}</div>{% endraw %}



### JS/JSX에서 컴포넌트 이름 규칙 지정(casing) <sup data-p="b">매우 추천함</sup>

**JS에서의 컴포넌트 이름은 PascalCase 규칙으로 작성되어야 하지만, `Vue.component`를 통해 전역 컴포넌트만 등록하는 간단한 어플리케이션의 경우, kebab-case로 작성될 수 있습니다.**
{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

JavaScript에서는, PascalCase는 클래스와 프로토타입 생성자의 표기법입니다. 특히 이는 시각적으로 더 잘 구분됩니다. 또한 Vue 컴포넌트는 인스턴스를 갖고 있으며, 그래서 PascalCase를 쓰는 것이 좀 더 좋은 생각입니다. PascalCase로 작성하여, 얻은 이점 덕분에, JSX(와 템플릿)내에서 PascalCase로 작성하는 것은 코드를 독해하는 사람에게 컴포넌트와 HTML 요소를 좀 더 쉽게 구분할 수 있게 만들어줍니다.
그러나 `Vue.component`를 통해 전역 컴포넌트만 정의한 어플리케이션에서는, 저희는 kebab-case를 대신에 사용하는 것을 권장합니다. 이유는 다음과 같습니다:
- 드물게 전역 컴포넌트가 JavaScript에서 참조될 수 있기에, JavaScript 표기법을 지키는 것은 그렇게 좋은 생각은 아닐 수 있습니다.
- 어플리케이션 안에는 항상 많은 내부 DOM 템플릿을 포함하고 있고, 템플릿에서는 [**반드시** kebab-case가 사용되어야 합니다](#Component-name-casing-in-templates-strongly-recommended).
{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
Vue.component('myComponent', {
  // ...
})
```

``` js
import myComponent from './MyComponent.vue'
```

``` js
export default {
  name: 'myComponent',
  // ...
}
```

``` js
export default {
  name: 'my-component',
  // ...
}
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
Vue.component('MyComponent', {
  // ...
})
```

``` js
Vue.component('my-component', {
  // ...
})
```

``` js
import MyComponent from './MyComponent.vue'
```

``` js
export default {
  name: 'MyComponent',
  // ...
}
```
{% raw %}</div>{% endraw %}



### 전체 이름 컴포넌트 이름 <sup data-p="b">매우 추천함</sup>

 **컴포넌트 이름은 약어보다 전문(full word)을 선호해야 합니다.**

편집기의 자동완성 기능을 사용하면 긴 변수명을 매우 간편하게 작성할 수 있으므로 긴 변수명을 사용하는데 드는 비용은 거의 없는데 반해, 긴 변수명이 제공하는 명확성은 프로그래밍에 있어 매우 중요하기 때문에 컴포넌트 이름은 약어보다 전문을 사용하는 것이 권장됩니다. 특히 관례적으로 사용되지 않는 특수한 약어는 언제나 피하는 것이 강력하게 권장됩니다.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

```
components/
|- SdSettings.vue
|- UProfOpts.vue
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

```
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```
{% raw %}</div>{% endraw %}



### Prop 이름 규칙 지정(casing) <sup data-p="b">매우 추천함</sup>

**Prop 이름은 선언하는 경우에는 항상 camelCase를 사용해야 하고, templates와 [ JSX ](../guide/render-function.html#JSX)에서는 kebab-case를 사용해야 합니다.**

우리는 단순히 각 언어의 규칙을 따릅니다. JavaScript 코드 내부에서는 camelCase, HTML 코드 내부에서는 kebab-case가 더 자연스럽습니다.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
props: {
  'greeting-text': String
}
```

{% codeblock lang:html %}
<WelcomeMessage greetingText="hi"/>
{% endcodeblock %}
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
props: {
  greetingText: String
}
```

{% codeblock lang:html %}
<WelcomeMessage greeting-text="hi"/>
{% endcodeblock %}
{% raw %}</div>{% endraw %}



### 다중 속성 엘리먼트 <sup data-p="b">매우 추천함</sup>

**Elements with multiple attributes should span multiple lines, with one attribute per line.**

In JavaScript, splitting objects with multiple properties over multiple lines is widely considered a good convention, because it's much easier to read. Our templates and [JSX](../guide/render-function.html#JSX) deserve the same consideration.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
```

``` html
<MyComponent foo="a" bar="b" baz="c"/>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```

``` html
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```
{% raw %}</div>{% endraw %}



### 템플릿에서 단순한 표현식 <sup data-p="b">매우 추천함</sup>

**Component templates should only include simple expressions, with more complex expressions refactored into computed properties or methods.**

Complex expressions in your templates make them less declarative. We should strive to describe _what_ should appear, not _how_ we're computing that value. Computed properties and methods also allow the code to be reused.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
{{
  fullName.split(' ').map(function (word) {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<!-- In a template -->
{{ normalizedFullName }}
```

``` js
// The complex expression has been moved to a computed property
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```
{% raw %}</div>{% endraw %}



### 단순한 계산된 속성 <sup data-p="b">매우 추천함</sup>

**Complex computed properties should be split into as many simpler properties as possible.**

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

Simpler, well-named computed properties are:

- __Easier to test__

  When each computed property contains only a very simple expression, with very few dependencies, it's much easier to write tests confirming that it works correctly.

- __Easier to read__

  Simplifying computed properties forces you to give each value a descriptive name, even if it's not reused. This makes it much easier for other developers (and future you) to focus in on the code they care about and figure out what's going on.

- __More adaptable to changing requirements__

  Any value that can be named might be useful to the view. For example, we might decide to display a message telling the user how much money they saved. We might also decide to calculate sales tax, but perhaps display it separately, rather than as part of the final price.

  Small, focused computed properties make fewer assumptions about how information will be used, so require less refactoring as requirements change.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```
{% raw %}</div>{% endraw %}



### 속성 값에 따옴표 <sup data-p="b">매우 추천함</sup>

**HTML 속성 값이 존재할 때에는 반드시 따옴표 구문을 사용해야 합니다. (작은 따옴표나 큰 따옴표, 혹은 JS에서 쓰이지 않는 것이라도)**

공백이 필요없는 속성 값에 따옴표를 쓰지 않아도 되지만, _공백을 쓰지 않는 습관_은 가독성을 떨어뜨립니다.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<input type=text>
```

``` html
<AppSidebar :style={width:sidebarWidth+'px'}>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<input type="text">
```

``` html
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```
{% raw %}</div>{% endraw %}



### 축약형 디렉티브 <sup data-p="b">매우 추천함</sup>

** 축약형 디텍티브( ` v-bind:` 의 경우 `:`, `v-on:`의 경우 `@` 및 `v-slot`의 경우 `#`)는 모든 문서에 걸쳐서 일관되게 사용해야 합니다. 즉, 모든 문서에서 항상 사용하거나 전혀 사용하지 않아야 합니다.**

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<input
  v-bind:value="newTodoText"
  :placeholder="newTodoInstructions"
>
```

``` html
<input
  v-on:input="onInput"
  @focus="onFocus"
>
```

``` html
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
>
```

``` html
<input
  v-bind:value="newTodoText"
  v-bind:placeholder="newTodoInstructions"
>
```

``` html
<input
  @input="onInput"
  @focus="onFocus"
>
```

``` html
<input
  v-on:input="onInput"
  v-on:focus="onFocus"
>
```

``` html
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template v-slot:footer>
  <p>Here's some contact info</p>
</template>
```

``` html
<template #header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```
{% raw %}</div>{% endraw %}



## 우선순위 C 규칙: 추천함 (선택의 혼란 또는 판단 오버헤드 최소화)



### 컴포넌트/인스턴스 옵션 순서 <sup data-p="c">추천함</sup>

**컴포넌트/인스턴스 옵션의 순서는 언제나 일정한 순서로 정렬되어야 합니다.**

아래는 권장하는 컴포넌트 옵션 순서입니다. 유형별로 나누어 놓았으므로, 플러그인으로 추가되는 새로운 옵션들 역시 이에 맞추어 정렬하면 됩니다.

1. **사이드 이펙트(Side Effects)** (컴포넌트 외부에 효과가 미치는 옵션)
  - `el`

2. **전역 인지(Global Awareness)** (컴포넌트 바깥의 지식을 필요로 하는 옵션)
  - `name`
  - `parent`

3. **컴포넌트 유형(Component Type)** (컴포넌트의 유형을 바꾸는 옵션)
  - `functional`

4. **템플릿 변경자(Template Modifiers)** (템플릿이 컴파일되는 방식을 바꾸는 옵션)
  - `delimiters`
  - `comments`

5. **템플릿 의존성(Template Dependencies)** (템플릿에 이용되는 요소들을 지정하는 옵션)
  - `components`
  - `directives`
  - `filters`

6. **컴포지션(Composition)** (다른 컴포넌트의 속성을 가져와 합치는 옵션)
  - `extends`
  - `mixins`

7. **인터페이스(Interface)** (컴포넌트의 인터페이스를 지정하는 옵션)
  - `inheritAttrs`
  - `model`
  - `props`/`propsData`

8. **지역 상태(Local State)** (반응적인 지역 속성들을 설정하는 옵션)
  - `data`
  - `computed`

9. **이벤트(Events)** (반응적인 이벤트에 의해 실행되는 콜백을 지정하는 옵션)
  - `watch`
  - 라이프사이클 이벤트 (호출 순서대로 정렬)
    - `beforeCreate`
    - `created`
    - `beforeMount`
    - `mounted`
    - `beforeUpdate`
    - `updated`
    - `activated`
    - `deactivated`
    - `beforeDestroy`
    - `destroyed`

10. **비반응적 속성(Non-Reactive Properties)** (시스템의 반응성과 관계없는 인스턴스 속성을 지정하는 옵션)
  - `methods`

11. **렌더링(Rendering)** (컴포넌트 출력을 선언적으로 지정하는 옵션)
  - `template`/`render`
  - `renderError`



### 엘리먼트 속성 순서 <sup data-p="c">추천함</sup>

**엘리먼트 및 컴포넌트의 속성은 언제나 일정한 순서로 정렬되어야 합니다.**

아래는 권장하는 엘리먼트 속성 순서입니다. 유형별로 나누어 놓았으므로, 커스텀 속성이나 directive 역시 이에 맞추어 정렬하면 됩니다.

1. **정의(Definition)** (컴포넌트 옵션을 제공하는 속성)
  - `is`

2. **리스트 렌더링(List Rendering)** (같은 엘리먼트의 변형을 여러 개 생성하는 속성)
  - `v-for`

3. **조건부(Conditionals)** (엘리먼트가 렌더링 되는지 혹은 보여지는지 여부를 결정하는 속성)
  - `v-if`
  - `v-else-if`
  - `v-else`
  - `v-show`
  - `v-cloak`

4. **렌더 변경자(Render Modifiers)** (엘리먼트의 렌더링 방식을 변경하는 속성)
  - `v-pre`
  - `v-once`

5. **전역 인지(Global Awareness)** (컴포넌트 바깥의 지식을 요구하는 속성)
  - `id`

6. **유일한 속성(Unique Attributes)** (유일한 값을 가질 것을 요구하는 속성)
  - `ref`
  - `key`
  - `slot`

7. **양방향 바인딩(Two-Way Binding)** (바인딩과 이벤트를 결합하는 속성)
  - `v-model`

8. **기타 속성** (따로 언급하지 않은 속성들)

9. **이벤트(Events)** (컴포넌트 이벤트 리스너를 지정하는 속성)
  - `v-on`

10. **내용(Content)** (엘리먼트의 내용을 덮어쓰는 속성)
  - `v-html`
  - `v-text`



### 컴포넌트/인스턴스 옵션 간 빈 줄 <sup data-p="c">추천함</sup>

**You may want to add one empty line between multi-line properties, particularly if the options can no longer fit on your screen without scrolling.**

When components begin to feel cramped or difficult to read, adding spaces between multi-line properties can make them easier to skim again. In some editors, such as Vim, formatting options like this can also make them easier to navigate with the keyboard.

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
props: {
  value: {
    type: String,
    required: true
  },

  focused: {
    type: Boolean,
    default: false
  },

  label: String,
  icon: String
},

computed: {
  formattedValue: function () {
    // ...
  },

  inputClasses: function () {
    // ...
  }
}
```

``` js
// No spaces are also fine, as long as the component
// is still easy to read and navigate.
props: {
  value: {
    type: String,
    required: true
  },
  focused: {
    type: Boolean,
    default: false
  },
  label: String,
  icon: String
},
computed: {
  formattedValue: function () {
    // ...
  },
  inputClasses: function () {
    // ...
  }
}
```
{% raw %}</div>{% endraw %}



### 싱글 파일 컴포넌트 최상위 엘리먼트 순서 <sup data-p="c">추천함</sup>

**[Single-file components](../guide/single-file-components.html) should always order `<script>`, `<template>`, and `<style>` tags consistently, with `<style>` last, because at least one of the other two is always necessary.**

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<style>/* ... */</style>
<script>/* ... */</script>
<template>...</template>
```

``` html
<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>
```

``` html
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```
{% raw %}</div>{% endraw %}



## 우선순위 D 규칙: 주의 요함 (잠재적인 위험을 내포한 패턴)



### `key`가 없는 `v-if`/`v-if-else`/`v-else` <sup data-p="d">주의 요함</sup>

**It's usually best to use `key` with `v-if` + `v-else`, if they are the same element type (e.g. both `<div>` elements).**

By default, Vue updates the DOM as efficiently as possible. That means when switching between elements of the same type, it simply patches the existing element, rather than removing it and adding a new one in its place. This can have [unintended consequences](https://jsfiddle.net/chrisvfritz/bh8fLeds/) if these elements should not actually be considered the same.

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<div v-if="error">
  Error: {{ error }}
</div>
<div v-else>
  {{ results }}
</div>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<div
  v-if="error"
  key="search-status"
>
  Error: {{ error }}
</div>
<div
  v-else
  key="search-results"
>
  {{ results }}
</div>
```
{% raw %}</div>{% endraw %}



### `scoped`에서 엘리먼트 셀렉터 사용 <sup data-p="d">주의 요함</sup>

**Element selectors should be avoided with `scoped`.**

Prefer class selectors over element selectors in `scoped` styles, because large numbers of element selectors are slow.

{% raw %}
<details>
<summary>
  <h4>자세한 설명</h4>
</summary>
{% endraw %}

To scope styles, Vue adds a unique attribute to component elements, such as `data-v-f3f3eg9`. Then selectors are modified so that only matching elements with this attribute are selected (e.g. `button[data-v-f3f3eg9]`).

The problem is that large numbers of [element-attribute selectors](http://stevesouders.com/efws/css-selectors/csscreate.php?n=1000&sel=a%5Bhref%5D&body=background%3A+%23CFD&ne=1000) (e.g. `button[data-v-f3f3eg9]`) will be considerably slower than [class-attribute selectors](http://stevesouders.com/efws/css-selectors/csscreate.php?n=1000&sel=.class%5Bhref%5D&body=background%3A+%23CFD&ne=1000) (e.g. `.btn-close[data-v-f3f3eg9]`), so class selectors should be preferred whenever possible.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` html
<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` html
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```
{% raw %}</div>{% endraw %}



### 부모-자식 간 의사소통 <sup data-p="d">주의 요함</sup>

**Props and events should be preferred for parent-child component communication, instead of `this.$parent` or mutating props.**

An ideal Vue application is props down, events up. Sticking to this convention makes your components much easier to understand. However, there are edge cases where prop mutation or `this.$parent` can simplify two components that are already deeply coupled.

The problem is, there are also many _simple_ cases where these patterns may offer convenience. Beware: do not be seduced into trading simplicity (being able to understand the flow of your state) for short-term convenience (writing less code).

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: '<input v-model="todo.text">'
})
```

``` js
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    removeTodo () {
      var vm = this
      vm.$parent.todos = vm.$parent.todos.filter(function (todo) {
        return todo.id !== vm.todo.id
      })
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="removeTodo">
        X
      </button>
    </span>
  `
})
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})
```

``` js
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="$emit('delete')">
        X
      </button>
    </span>
  `
})
```
{% raw %}</div>{% endraw %}



### 전역 상태 관리 <sup data-p="d">주의 요함</sup>

**전역 상태 관리에는 `this.$root` 나 글로벌 이벤트 버스(global event bus)보다는 [Vuex](https://github.com/vuejs/vuex)를 이용하는 것이 좋습니다.**

전역 상태 관리에 `this.$root`나 [글로벌 이벤트 버스](https://vuejs.org/v2/guide/migration.html#dispatch-and-broadcast-replaced)를 이용하는 것은 아주 단순한 경우에는 편리할 수 있지만 대부분의 Application에는 부적절합니다. Vuex는 상태를 관리하는 중앙 저장소를 제공하는 것만이 아니라 상태 변화를 체계화하고, 추적하며, 디버깅을 용이하게 하는 도구들을 제공합니다.

{% raw %}</details>{% endraw %}

{% raw %}<div class="style-example example-bad">{% endraw %}
#### 나쁨

``` js
// main.js
new Vue({
  data: {
    todos: []
  },
  created: function () {
    this.$on('remove-todo', this.removeTodo)
  },
  methods: {
    removeTodo: function (todo) {
      var todoIdToRemove = todo.id
      this.todos = this.todos.filter(function (todo) {
        return todo.id !== todoIdToRemove
      })
    }
  }
})
```
{% raw %}</div>{% endraw %}

{% raw %}<div class="style-example example-good">{% endraw %}
#### 좋음

``` js
// store/modules/todos.js
export default {
  state: {
    list: []
  },
  mutations: {
    REMOVE_TODO (state, todoId) {
      state.list = state.list.filter(todo => todo.id !== todoId)
    }
  },
  actions: {
    removeTodo ({ commit, state }, todo) {
      commit('REMOVE_TODO', todo.id)
    }
  }
}
```

``` html
<!-- TodoItem.vue -->
<template>
  <span>
    {{ todo.text }}
    <button @click="removeTodo(todo)">
      X
    </button>
  </span>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: mapActions(['removeTodo'])
}
</script>
```
{% raw %}</div>{% endraw %}



{% raw %}
<script>
(function () {
  var enforcementTypes = {
    none: '<span title="There is unfortunately no way to automatically enforce this rule.">self-discipline</span>',
    runtime: 'runtime error',
    linter: '<a href="https://github.com/vuejs/eslint-plugin-vue#eslint-plugin-vue" target="_blank" rel="noopener noreferrer">plugin:vue/recommended</a>'
  }
  Vue.component('sg-enforcement', {
    template: '\
      <span>\
        <strong>Enforcement</strong>:\
        <span class="style-rule-tag" v-html="humanType"/>\
      </span>\
    ',
    props: {
      type: {
        type: String,
        required: true,
        validate: function (value) {
          Object.keys(enforcementTypes).indexOf(value) !== -1
        }
      }
    },
    computed: {
      humanType: function () {
        return enforcementTypes[this.type]
      }
    }
  })

  // new Vue({
  //  el: '#main'
  // })
})()
</script>
{% endraw %}
