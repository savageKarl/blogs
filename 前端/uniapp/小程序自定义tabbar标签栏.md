# 小程序自定义 tabbar 标签栏

业务项目使用 uniapp 来开发小程序，在标签栏的时候需要使用自定义的图标，小程序的tabbar机制不允许，所以开发自定义标签栏。

遇到一个复杂的需求，项目添加商城系统，从主包的标签栏上进去分包商城系统，是否显示图标入口还要由后台接口控制，分包商城系统也要标签栏。

## 主包自定义标签栏

原理：自己封装标签栏组件，在每个标签栏页面引入，也使用官方的tabbar机制，在 pages.json 里面配置（原生小程序的是 app.json 页面），在点击标签栏的时候，首先判断是否分包商城系统入口icon，如果是就使用 navigateTo 跳转至分包`index.vue`（分包的首页）页面，如果不是，就使用 switchTab 切换标签栏页面。

## 分包标签栏

小程序并不支持分包tabbar，所以需要自己封装一个分包使用的标签栏。

原理：新建一个 `index.vue`（分包的首页）页面，将所有标签栏页面当作组件引入，然后封装 tabbar 组件，`index.vue`页面使用`currentPage`变量维护当前是哪个标签栏页面，然后将`currentPage`传递给 tabbar 组件，让 tabbar 改变激活状态，tabbar 被点击时，tabbar 发送事件，将 tabbar 列表的 pageName 传递给 index.vue 页面，index.vue 页面用 currentPage 跟 pageName 判断，用 v-if 指令控制当前标签栏页面的显示或隐藏。 











