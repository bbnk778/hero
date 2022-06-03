Not Fork！

## v2ray-heroku

[![](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/qq198812/Hreoku.git)

## 注意事项
### heroku上部署v2ray
- [x] 支持VMess和VLESS两种协议
- [x] 使用v2ray最新版构建

### CloudFlare Workers反代代码
<details>
<summary>CloudFlare Workers单账户反代代码</summary>

```js
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers单双日轮换反代代码</summary>

```js
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Pages单账户反代代码</summary>

```js
export default {
  async fetch(request, env) {
    let url = new URL(request.url);
    if (url.pathname.startsWith('/')) {
      url.hostname = 'app0.herokuapp.com'
      let new_request = new Request(url, request);
      return fetch(new_request);
    }
    return env.ASSETS.fetch(request);
  },
};
```
</details>

<details>
<summary>CloudFlare Pages单双日轮换反代代码</summary>

```js
export default {
  async fetch(request, env) {
    const day1 = 'app0.herokuapp.com'
    const day2 = 'app1.herokuapp.com'
    let url = new URL(request.url);
    if (url.pathname.startsWith('/')) {
      let day = new Date()
      if (day.getDay() % 2) {
        url.hostname = day1
      } else {
        url.hostname = day2
      }
      let new_request = new Request(url, request);
      return fetch(new_request);
    }
    return env.ASSETS.fetch(request);
  },
};
```
</details>



