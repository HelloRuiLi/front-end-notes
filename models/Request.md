## Request

- 在发起 `request` 时, 可能会需要 stringify data `data: JSON.stringify(loginBody)`
- 可以用 `qs` 来帮助 encode data
 
```js
import qs from 'qs';
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```