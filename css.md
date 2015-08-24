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

## 五个常用的SCSS函数
### 1. 颜色stack
```scss
$color-stack :
	(group: blue, id: normal, color: #4399EE),
	(group: blue, id: darker, color: #B3E5FC),
	(group: blue, id: lighter, color: #03A9F4),
	(group: blue, id: dark, color: #448AFF),
	(group: blue, id: light, color: #00BCD4);

@function color ($ground, $shade:normal, $transparency:1) {
	@each $color in $color-stack{
    $color-group: map-get($color, group);
    $color-shade: map-get($color, id);
    @if($group == map-get($color, group) and $shade == map-get($color, id)){
      @return rgba(map-get($color, color), $transparency);
    }
  }
}

body {
	background-color: color(blue);
	color: color(blue, light, 0.5);
	p {
		color: color(blue, dark);
	}
}
```