# 레이아웃

주로 display `flex` 속성을 이용해서 기본 레이아웃에 대한 구성을 하고
메인축은 `justyfy`, 교차축은 `align`을 이용함.

![dream1](https://user-images.githubusercontent.com/18409941/196315987-17539789-76e6-40c8-bd8d-bad3ee85e496.png)
_min-width: 768px_

# :hover

메뉴 마우스 오버시 일어나는 스타일은 `:hover`를 이용하는데 `Pseudo-classes`라고 함.

# icon

https://fontawesome.com/ 에서 제공하는 아이콘들을 이용.
React 에서 사용할 때는 아래와 같이 사용 함.

`@fortawesome/free-solid-svg-icons` 에는 일부 아이콘만 제공하는거같음.

```html
<script
  src="https://kit.fontawesome.com/2398f113df.js"
  crossorigin="anonymous"
></script>

...
<i class="fa-brands fa-square-pied-piper"></i>
...
```

```tsx
import { faGears } from "@fortawesome/free-solid-svg-icons";
return (
  <>
    ...
    <FontAwesomeIcon icon={faGears} />
    ...
  </>
);
```

# @media

width가 768px보다 작아졌을때 기본 상태에서는 메뉴가 사라지게 처리
메인축이 달라져야하므로 `flex-direction`

![dream2](https://user-images.githubusercontent.com/18409941/196315989-e352f309-7fbe-4d13-929c-21d155bf7995.png)
_max-width: 768px default_

# toggle

```js
const hamburger = document.querySelector(".hamburger");
const nav = document.querySelector(".nav");
const menu = document.querySelector(".menu");
const icon = document.querySelector(".icon");

hamburger.addEventListener("click", () => {
  nav.classList.toggle("active");
  menu.classList.toggle("active");
  icon.classList.toggle("active");
});
```

![dream3](https://user-images.githubusercontent.com/18409941/196315992-1313b54b-b066-419e-94d0-b117c94d5123.png)
_max-width: 768px active_

## 코드리뷰

1. 메뉴는 늘어날 수 있기 때문에 `flex: 1` 로고, 아이콘은 영역이 고정이므로 `flex: none`
2. 아이콘의 경우 디자인이 특정 px로 시안이 나오므로 넓이값을 먼저 주고 `flex, align`등을 이용하여 정렬

   ![css1](https://user-images.githubusercontent.com/18409941/196352182-b986d673-7f75-4f4c-858b-668680cd7e97.png)
   _수정사항_

   ![css2](https://user-images.githubusercontent.com/18409941/196352191-685d9c13-0be3-4a60-87b3-738b2a4f31fc.png)
   _flex:none_
