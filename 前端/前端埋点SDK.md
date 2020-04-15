# 前端埋点SDK

> **目前主要在chrome 浏览器中测试，其他浏览器没有经过测试**。

## SDK 结构

### Track模块

> `Track`模块是埋点的基础。定义了一个 `Track`类。埋点过程从这个实例化这个类开始。

#### `API`

#####  `config`属性

> * 类型:  `TrackConfig`
> * 说明：创建实例时传入的配置和默认配置的合并。
>
> ```typescript
> interface TrackConfig {
>   // 服务端接口地址
>   api_host: string;
> 	// 是否开启自动埋点, 可选值: true || false
>   // 默认: true
>   auto_track: boolean;
>   // 是否开启自动收集PV, 可选值: true || false
>   // 默认: true
>   track_pageview: boolean;
>   // 是否开启自动统计输入框输入时长, 可选值: true || false
>   // 默认: true
>   track_input_time: boolean;
>   // 设定哪些HTML元素不触发埋点事件
>   track_elements_blacklist: string[];
>   // 发送数据请求类型 可选值: "XHR" || "Image"
>   // 默认: "XHR"
>   request_type: string;
>   // 设置哪些属性不可上传
>   track_property_blacklist: string[];
>   // 设置跳转元素发起请求的超时时间，单位: ms
>   // 默认: 300ms
>   track_link_timeout: number;
>   // 埋点实例在全局的变量名称
>   lib_instance_name: string;
>   // 是否开启敏感数据的过滤, 可选值: true || false
>   // 默认: true
>   filter_sensitive_data: boolean;
>   // 是否是SPA应用 可选值: true || false
>   // 默认: true
>   track_single_page: boolean;
>   // 是否开启批量发送 
>   // 可选值: true || false || { max_length, timeout, interval}
>   // 	max_lengt: number 每一次批量发送的条数
>   //  timeout: number 	每次发送的超时时间
>   // 	interval: number 	多长时间发送一次
>   // 默认: { max_length: 10, timeout:5000, interval: 8000}
>   batch_send: boolean | batchSendConfig;
>   
>   // 其他
>   [key: string]: any;
> } 
> ```
>
> 

##### `track(event_name, props, options, callback)`

> 核心方法。触发收集
>
> * `event_name`:  `string` 	收集的事件名称
> * `props`:  `object`               收集的数据
> * `options`: `object`            辅助选项
> * `callback`:  `function`     请求发送后的回调
>
> ```typescript
> // 手动埋点 触发一个 buy 事件
> const track = new Track({...})
> track.track("buy", {name: "Apple Mac", price: 300000}, {}, () => {});
> ```
>
> 



##### `auto_track()`

> 开启自动埋点

##### `add_public_data(data)`

> * `data`: `object`	需要添加的数据
>
> 对于在实例化的时候还没有。在之后的某个点拿到后，每次触发埋点都需要上传的数据，可以通过该方法添加到实例的全局数据中。

##### `get_config(key)`

> * `key`: `string`  需要获得`key`属性
>
> 获取指定的配置项

##### `track_pageview(page)`

> * `page`:  `string` 页面地址 默认是当前页
>
> 触发 `$pageview`事件。

##### `track_input_time()`

> 触发 `$input_time事件。

##### `track_link(querySelector, event_name, props, callback)`

> * `querySelector`: `string` | `Element` 需要触发的 `dom`
> * `event_name`: `string`  事件名称
> * `props`: `object`             事件数据
> * `callback`: `function`  请求发送后的回调
>
> 触发 `a` 标签的`click`事件

##### `track_form(querySelector, event_name, props, callback)`

> * `querySelector`: `string` | `Element` 需要触发的 `dom`
> * `event_name`: `string`  事件名称
> * `props`: `object`             事件数据
> * `callback`: `function`  请求发送后的回调
>
> 触发 `form` 标签的`submit`事件

### 自动埋点模块

> 自动埋点模块是一个对象。

#### `API`

##### `init(instance)`

> * `instance`: `Track` `track` 实例
>
> 初始化自动埋点

##### `track_event(instance，event，callback)`

> * `instance`: `Track` `track` 实例
> * `event`: `Event` 事件对象
> * `callback`: `function`  请求发送后的回调
>
> 自动埋点中触发埋点事件的逻辑执行函数。

### 特殊标签模块

> 对 一些操作会刷新当前页面的`HTML`元素的特殊处理模块。

#### `class 对象`

##### `DomTrack`

> 处理特殊模块的父类。其他特殊模块可以集成此类。

##### `LinkTrack`

> 处理`a`标签`click`事件的`class`。

##### `FormTrack`

> 处理 `form`标签`submit`事件的`class`。



### 辅助工具模块

> 定义了一些常用方法

### history模块

> 处理监听页面路由变化。用于 `SPA`的路由变化监听。

### store模块

> 数据存储模块，用于操作 `localStorage`。在`localStorage`基础上扩展 一些方法。

### 基础数据模块

> 处理一些在当前环境下某些执行过一次后，就不会变化的值的模块。比如： 用户的`userAgent`、`screen`对象

### 数据请求模块

> 和服务端数据交互的模块，定义了数据请求方式。

