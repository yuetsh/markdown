# css
## vertical-align的用处
```html
<div class="vcp-title">
    <img src="assets/images/icons/{{icon}}" alt=""/>
    <span class="font-20" ng-bind="title"></span>
</div>
```
```scss
.vcp-title {
  width: 110px;
  border-bottom: 2px solid $pure-mainblue;
  img {
    vertical-align: middle;
  }
  span {
    color: $font-color;
    line-height: 36px;
    vertical-align: middle;
  }
}
```
内联元素的垂直居中：比如左边一个img，右边一个span的时候。