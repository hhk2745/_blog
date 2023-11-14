# 쿠키
HTTP 쿠키(웹 쿠키, 인터넷 쿠키, 브라우저 쿠키, 간단히 `쿠키`)를 통해 웹서버는 상태 정보(ex: 온라인 상점의 장바구니, 로그인, 방문기록, 입력폼 저장) 등 사용자가 입력한 정보를 차후 사용을 위해 저장하는 데에도 사용 가능함.
`인증쿠키`는 일반적으로 웹 서버에서 사용자가 어떤 계정으로 로그인했는지 인증하는데 사용됨. 
관련 자료 
[스크립팅](https://en.wikipedia.org/wiki/Cross-site_scripting) 및 [사이트 간 요청 위조](https://en.wikipedia.org/wiki/Cross-site_request_forgery)

# 웹 스토리지 API
Web Storage API는 브라우저가 쿠키를 사용하는 것보다 직관적인 방식으로 key-value 를 저장할 수 있는 메커니즘을 제공함.

Web Storage 내의 두 가지 메커니즘

* `sessionStorage` 페이지 세션 기간 동안 사용할 수 있는 각 출처에 대해 별도의 저장 영역을 유지함.(페이지 다시 로드 및 복원을 포함하여 브라우저가 열려 있는 동안).
  * 세션에 대한 데이터만 저장, 브라우저(또는 탭)가 닫힐 때까지 데이터가 저장됨.
  * 데이터는 서버로 전송되지 않음.
  * 저장 제한이 쿠키보다 큼(최대 5MB)

* `localStorage` 동일한 작업을 수행하지만 브라우저를 닫았다가 다시 열어도 지속됨.
  * 만료 날짜 없이 데이터를 저장하고 javascript를 통해서만 삭제되거나 브라우저 캐시/로컬에 저장된 데이터를 삭제함.
  * 저장 한도는 최대치를 갖고 있음.

```js
window.addEventListener('storage', (e)=>{
  // do something
})
```
대부분의 브라우저에서 다른 윈도우에서 로컬스토리지의 데이터를 변경했을 때 이벤트가 발생함. 같은 창에 이벤트를 추가하고 localStorage의 값을 변경해도 이벤트가 발생하지 않음.
자신의 window에서는 변경에 대한 상황을 알고 있을 것이라 판단 했을거라 예상함.

[example-index](https://mdn.github.io/dom-examples/web-storage/)
[example-event](https://mdn.github.io/dom-examples/web-storage/event.html)

example-event 페이지를 열어두고 example-index 페이지에서 값을 변경하면 event 페이지에서 이벤트를 감지해서 화면에 출력함.
