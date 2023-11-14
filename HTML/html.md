# HTML 공간 분할

HTML의 모든 요소는 해당 요소가 웹 브라우저에 어떻게 보이는가를 결정짓는 display 속성을 가집니다.
대부분의 HTML 요소는 이러한 display 속성값으로 다음 두 가지 중 하나를 가집니다.

1. 블록(block)
2. 인라인(inline)

## 블록(block) 타입 요소

display 속성값이 block인 요소는 언제나 새로운 라인에서 시작하며, 해당 라인의 모든 너비를 차지함.

```html
<p style="border: 3px solid red">inline</p>
```

대표적인 block 타입 >> p, div, h, ul, ol, form

### div

div는 다른 HTML 요소들을 하나로 묶는데 자주 사용함.

```html
<div style="background-color:lightgrey; color:green; text-align:center">
  <h1>div요소를 이용한 스타일 적용</h1>

  <p>
    이렇게 div요소로 여러 요소들을 묶은 다음에 style 속성과 클래스 등을 이용하여
    한 번에 스타일을 적용할 수 있습니다.
  </p>
</div>
```

## 인라인(inline) 타입 요소

display 속성값이 인라인(inline)인 요소는 새로운 라인에서 시작하지 않음.
또한, 요소의 너비도 해당 라인 전체가 아닌 해당 HTML 요소의 내용(content)만큼만 차지함.

```html
<p>
  <span style="background-color:grey; color:orange">span태그</span>는 display
  속성값이 인라인인 요소입니다.
</p>
```

대표적인 inline 타입 >> span, a, img

## HTML5 레이아웃 시멘틱 태그(semantic tag)

- header: HTML문서나 섹션(section) 부분에 대한 헤더(header)를 정의.
- nav: 탐색 링크를 정의.
- section: 섹션(section) 부분을 정의.
- article: 독릭적인 하나의 글(article) 부분을 정의.
- aside: 페이지 부분 이외의 콘텐츠(content)를 정의.
