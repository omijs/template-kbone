# omi-kbone

使用 [omis](https://github.com/Tencent/omi/tree/master/packages/omis) 多端开发(小程序和Web)，基于 [kbone](https://github.com/wechat-miniprogram/kbone) 的 element 和 render。

## 一套语法多端运行

```jsx
import { h } from 'omis'
import './index.css'

const Counter = (props, store) => {
  return (
    <div>
      <button onClick={store.sub}>-</button>
      <span>{store.count}</span>
      <button onClick={store.add}>+</button>

      <div onClick={store.clickHandle}>跳转</div>
    </div>
  )
}

Counter.store = _ => {
  return {
    count: 1,
    add() {
      this.count++
      _.update()
    },
    sub() {
      this.count--
      _.update()
    },
    clickHandle() {
      if ("undefined" != typeof wx && wx.getSystemInfoSync) {
        wx.navigateTo({
          url: '../log/index?id=1'
        })
      } else {
        location.href = 'log.html'
      }
    }
  }
}

export default Counter
```

注意这里 css 在小程序和web里都是全局作用，小程序没法做到 scoped style，在 web 里是可以做到 scoped，只需要这样：

```js
Counter.css = require('./_index.css')
```

使用下划线前缀并且赋值便可。

## 快速开始

```js
npm i omi-cli -g
omi init-kbone my-app
cd my-app
npm start        //开发小程序
npm run web      //开发 web
npm run build    //发布 web
```

> 也支持一条命令 `npx omi-cli init-kbone my-app` (npm v5.2.0+)

## 目录说明

```
├─ build
│  ├─ mp     //微信开发者工具指向的目录，用于生产环境
│  ├─ web    //web 编译出的文件，用于生产环境
├─ config
├─ public
├─ scripts
├─ src
│  ├─ assets
│  ├─ components    //存放所有组件
│  ├─ log.js        //入口文件，会 build 成  log.html
│  └─ index.js      //入口文件，会 build 成  index.html
```


## 注意事项和已知问题

* 不要使用 bindtap，使用 onClick
* 图片请使用 cdn 地址或者 base64
* 如果要兼容 web，请用 HTML 和 CSS 标签，比如用 div，不用 view，不用 rpx 单位等
* DOM 上声明 data-xx 属性，在小程序 evt.target.dataset.xx 获取不到，请使用 function 传参方式（官方正在修复）

```jsx
<div class="todo-list">
  {todo.map(item => (
    <div>
      <div class="toggle" onClick={function () { this.toggle(item.id) }}></div>
      <text >{item.text} </text>
      <div class="delete" onClick={function () { this.deleteItem(item.id) }}></div>
    </div>
  ))}
</div>
```

## License

MIT 
