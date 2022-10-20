# 什么是 BEM 命名规范

BEM 是块（block）、元素（element）和修饰符（modfier）的简写，是 HTML 和 CSS 中 class 命令约定，这个约定让代码更加语义化，统一团队开发规范和方便维护。

个人喜欢命名约定的模式如下：

```css
/* 多个单词使用小驼峰命名 */
.block {}
.block_element {}
.block-modifier {}
.block_element-modifier {} 

```



## 块（block）

具有自身意义的独立实体。

例如：`header， container， footer， main， menu， btn， accordtion ` 。

## 元素（element）

块的一部分，没有独立的含义，并且在语义上与其块相关联。

例如：`menu_item, accordion_item，btn_text  `。

## 修饰符（modfier）

块或元素上的修饰符，使用它们来更改外观或行为。

例如：`menu_item-active, accordion_item-active, btn-disable, btn-success`。

# BEM 命名约定的好处

BEM的关键是，可以获得更多的描述和更加清晰的结构，从其名字可以知道某个标记的含义。于是，通过查看 HTML 代码中的 class 属性，就能知道元素之间的关联。

常规的命名法示例：

```html
<div class="article">
    <div class="body">
        <button class="button-primary"></button>
        <button class="button-success"></button>
    </div>
</div>
```

- 这种写法从 DOM 结构和类命名上可以了解每个元素的意义，但无法明确其真实的层级关系。在 css 定义时，也必须依靠层级选择器来限定约束作用域，以避免跨组件的样式污染。

使用了 BEM 命名方法的示例：

```html
<div class="article">
    <div class="article_body">
        <div class="tag"></div>
        <button class="article_button-primary"></button>
        <button class="article_button-success"></button>
    </div>
</div>
```

- 通过 BEM 命名方式，模块层级关系简单清晰，而且 css 书写上也不必作过多的层级选择。

-----

常规方式命名的 .siteSearch 的例子：

```html
<form class="siteSearch  full">
  <input type="text" class="field">
  <input type="Submit" value ="Search" class="button">
</form> 
```

使用 BEM 命名方式的例子；

```html
<form class="siteSearch  siteSearch-full">
  <input type="text" class="siteSearch_field">
  <input type="Submit" value ="Search" class="siteSearch_button">
</form>
```

这里能够清晰的看到有个叫 .siteSearch 的块，它的内部有一个叫 .siteSearch_field 的元素，并且 .siteSearch 还有另外一种形态叫 .siteSearch-full 。

# 一些使用 BEM 命名的例子

## button 按钮

```html
<a class="btn">
  <span class="btn_text">标准按钮</span>
</a>
<a class="btn btn-orange btn-big ">
  <span class="btn_price">￥3</span>
  <span class="btn_text">橙色按钮</span>
</a>
<a class="btn btn-blue btn-big ">
  <span class="btn_price">￥4</span>
  <span class="btn_text">蓝色按钮</span>
</a>
<a class="btn btn-green btn-big ">
  <span class="btn_price">￥9</span>
  <span class="btn_text">绿色按钮</span>
</a>
```

```css
/* 块 */
.btn {
  text-decoration: none;
  background-color: white;
  color: #888;
  border-radius: 5px;
  display: inline-block;
  margin: 10px;
  font-size: 18px;
  font-weight: 600;
  padding: 10px 5px;
}
/* 元素 */
.btn_price {
  color: #fff;
  padding-right: 12px;
  padding-left: 12px;
  margin-right: -10px;
  font-weight: 600;
  background-color: #333;
  opacity: .4;
  border-radius: 5px 0 0 5px;
}
/* 元素 */
.btn_text {
  padding: 0 10px;
  border-radius: 0 5px 5px 0;
}
/* 修饰符 */
.btn-big {
  font-size: 28px;
  padding: 10px;
  font-weight: 400;
}
/* 修饰符 */
.btn-blue {
  border-color: #0074d9;
  color: white;
  background-color: #0074d9;
}
/* 修饰符 */
.btn-orange {
  border-color: #FF4136;
  color: white;
  background-color: #FF4136;
}
/* 修饰符 */
.btn-green {
  border-color: #3d9970;
  color: white;
  background-color: #3d9970;
}
```

## accordion 图片手风琴

```html
<div class="accordion">
  <div class="accordion_item accordion_item-active">
    <img class="accordion_img" src="./imgs/1.jpg">
  </div>
  <div class="accordion_item">
    <img class="accordion_img" src="./imgs/2.jpg">
  </div>
  <div class="accordion_item">
    <img class="accordion_img" src="./imgs/3.jpg">
  </div>
  <div class="accordion_item">
    <img class="accordion_img" src="./imgs/4.jpg">
  </div>
  <div class="accordion_item">
    <img class="accordion_img" src="./imgs/5.jpg">
  </div>
  <div class="accordion_item">
    <img class="accordion_img" src="./imgs/6.jpg">
  </div>
</div>
```

```css
.accordion {
  margin: 100px auto;
  display: flex;
}
.accordion_item {
  width: 100px;
  height: 350px;
  float: left;
  transition: all 1s;
  cursor: pointer;
  overflow: hidden;
}
    
.accordion_item-active {
  width: 600px;
}
    
/* 
  制作图片手风琴需要主要的地方，由于图片大，需要缩放图片，
  为了显示图片的一部分且不变形这里只能设置height，不能设置width
*/
.accordion_img {
  height: 100%;
}
```

## recommendProducts 推荐商品

```html
<view class="recommendProducts">
  <view
    class="recommendProducts_item"
    v-for="(item, index) in recommendProducts"
    :key="index"
    @click="onGoToProductDetailPage(item.id)"
  >
    <view class="recommendProducts_img">
      <image :src="item.cover_url" mode="aspectFit" />
    </view>
    <view class="recommendProducts_text">{{item.title}}</view>
  </view>
</view>
```

```scss
.recommendProducts {
  display: flex;
  align-items: center;
  justify-content: space-around;
  border-bottom: 8px solid #eee;
  padding: 10px 0;
  background-color: #fff;

  &_item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  &_img {
    width: 120rpx;
    height: 120rpx;
  }

  &_text {
    font-size: 12px;
    padding: 10rpx 0;
    color: #666;
  }
}
```

## tabs 标签页

```html
<view class="tabs">
  <view
    class="tabs_item"
    v-for="(item, index) in tabsList"
    :key="index"
    @click="onChangeTab(index)"
  >
    <text 
      class="tabs_text"
      :class="{ 'tabs_text-active': index === currentTabIndex }"
    >
      {{ item.name }}
    </text>
  </view>
</view>
```

```scss
.tabs {
  display: flex;
  height: 40px;
  line-height: 40px;
  font-size: 14px;
  background-color: #fff;
  width: 100vw;

  &_item {
    flex: 1;
    text-align: center;
  }

  &_text {
    padding: 6px;

    &-active {
      $activeColor: #42b983;
      color: $activeColor;
      border-bottom:  3px solid $activeColor;
    }
  }
}
```

## productList 商品列表

```vue
<view class="productList">
  <view 
    class="productList_item" 
    v-for="(item, index) in products[currentTabType].list"
    :key="index"
    @click="onGoToProductDetailPage(item.id)"
  >
    <view class="productList_img"><image :src="item.cover_url" /></view>
    <view class="productList_title">{{ item.title }}</view>
    <view class="productList_price">￥{{ item.price }}</view>
    <view class="productList_collection">
      <u-icon name="star"></u-icon>{{ item.collects_count }}
    </view>
  </view>
</view>
```

```scss
.productList {
  display: flex;
  flex-wrap: wrap;
  // margin-bottom: 100rpx;
  background-color: #fff;
  padding: 10rpx;

  &_item {
    flex: 50%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
  }

  &_img {
    width: 300rpx;
    height: 300rpx;
  }
  
  &_title {
    width: 100%;
    text-align: center;
  }
  
  &_price {
    flex: 50%;
    text-align: right;
    color: red;
  }

  &_collection {
    flex: 50%;
    text-align: left;
    font-weight: 600;
    padding-left: 20rpx;
  }
}
```

## productCard 商品卡片

```vue
<view class="productCard">
  <view class="productCard_price">
    ￥{{products.specifications[0].price}}
  </view>
  <view class="productCard_title">{{products.title}}</view>
  <view class="productCard_expressExpenses">
    运费：{{products.expressExpenses}}
  </view>
  <view class="productCard_sales">销量{{products.sales}}</view>
</view>
```

## actionBar 操作栏

```vue
<view class="actionBar">
  <view class="actionBar_shopCart">
    <view class="actionBar_icon">
      <u-icon name="shopping-cart" size="20px"></u-icon>
    </view>
    <view class="actionBar_text">购物车</view>
  </view>
  <view class="actionBar_collection">
    <view class="actionBar_icon">
      <u-icon name="star" size="20px"></u-icon>
    </view>
    <view class="actionBar_text">收藏</view>
  </view>
  <view class="actionBar_addToCart">加入购物车</view>
  <view class="actionBar_buyNow">立即购买</view>
</view>
```

```scss
.actionBar {
  width: 100vw;
  height: 50px;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  padding: 8px 15px;
  background-color: #fff;
  text-align: center;
  align-items: center;
  justify-content: space-between;
  color: #fff

  &_shopCart {
    flex: 1;
  }

  &_collection {
    flex: 1;
  }

  &_addToCart {
    background: linear-gradient(to right,#ffd01e,#ff8917);;
    flex: 3;
    line-height: 40px;
    border-top-left-radius: 25px;
    border-bottom-left-radius: 25px;
  }

  &_buyNow {
    flex: 3;
    background: linear-gradient(to right,#ff6034,#ee0a24);
    line-height: 40px;
    border-top-right-radius: 25px;
    border-bottom-right-radius: 25px;
  }
}
```



















