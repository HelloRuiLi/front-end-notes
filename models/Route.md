## Route

### Browser API

* [Location](https://developer.mozilla.org/en-US/docs/Web/API/Location)

```json
{
  "hash": "",
  "search": "",   
  "pathname": "/models/Route.html",
  "port": "4000",
  "hostname": "127.0.0.1",
  "protocol": "http:",
  "origin": "http://localhost:4000",
  "href": "http://localhost:4000/models/Route.html"
}
```
* [History](https://developer.mozilla.org/en-US/docs/Web/API/History)

```json
{
  "length": 50,
  "scrollRestoration": "auto",
  "state": {
    "path": "http://localhost:4000/models/Route.html"
  }
}
```

### History

[Enhanced History](https://reacttraining.com/react-router/web/api/history)

* `browser history`
* `hash history`
* `memory history` (模拟，非浏览器环境(Node, ReactNative)适用, 可以用于 testing, server render 等场景


```typescript
interface IHistory {
  length: number // number of entries in the history stack
  action: string // current action(PUSH, REPLACE or POP),
  location: {
    pathname: string // path of the URL
    search: string // URL query string
    hash: string // URL hash fragment
    state: string // Only available in browser and memory history
  },
  push: (path: string, [state]) => {},
  replace: (path: string, [state]) => {}
  go: (n: number) => {}
  goBack: () => {} // Equivalent to go(-1)
  goForward: () => {} // Equivalent to go(1)
  block: (prompt) => {} // Prevents navigation
}
```
### URL

```
https:          //reacttraining.com/react-router/web/api/history?q=1&sortBy=name
|-- portcal --|  |--- hostname ---||--------- pathname --------||--- search ---|
```

#### URL Match

```js
const childRoute = {
  path: ':resources/action',
  component: Component,
};

const route = {
  path: '/resource',
  component: Component,
  childRoutes: [
    childRoute,
  ],
};

const matchRoutes = {
  route,
  childRoute,
};
```

#### 参数取得

```
/resource/:resourceId/action?q=1&sortBy=name
```

```js
const params = {
  resourceId: '1',
};

const query = {
  q: '1',
  sortBy: 'name',
};
```
* pathname 中的参数提取, 参考 [path-to-regexp](https://www.npmjs.com/package/path-to-regexp), `history` 有自己的内部实现。
* 当前 `history` 使用 [query-string](https://github.com/sindresorhus/query-string) 作为 search 的 parse, 不支持嵌套数据,
若必须, 需要[先 Stringify](https://github.com/sindresorhus/query-string#nesting)

```js
import querySring from 'query-string'; 
queryString.parse('?foo=bar');
//=> { foo: 'bar' }

const parsed = queryString.parse('?foo=bar');
queryString.stringify(parsed);
//=> 'foo=bar'
```
* 也可以使用 [qs 来 parse search](https://github.com/ljharb/qs), 它支持嵌套数据， 但是需要开启 `ignoreQueryPrefix` 或者对 query string 进行预处理

```js
import qs from 'qs';
const queryString = '?foo=123'.replace('?', '');
qs.parse(queryString);
//=> { foo: '123' }

qs.parse('?foo=123', { ignoreQueryPrefix: true })
//=> { foo: '123' }

qs.stringify({foo: 'bar', nested: {unicorn: 'cake'}})
//=> 'foo=bar&nested%5Bunicorn%5D=cake'
```
#### 多页状态的传递

* 明 `query`
* 暗 `session`, 在 `history` 中可以通过 `location.state` 存取
* 数据按需求进行传递

#### Direction

Url 的变化方向, `history` 中 `location.action` 区分 

* `POP` 
* `PUSH` 
* `REPLACE`

