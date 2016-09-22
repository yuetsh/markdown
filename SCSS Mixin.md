# SCSS Mixin
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

@function color ($group, $shade:normal, $transparency:1) {
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

### 2. 字体stack
> 感觉这个用的不多

```scss
$font-stack:
	(group: brandon, id: light, font: ('Brandon Grot W01 Light', san-serif ), weight: 200, style: normal),
	(group: brandon, id: light-italic, font: ('Brandon Grot W01 Light', san-serif ), weight: 200, style: italic),
	(group: brandon, id: regular, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: normal),
	(group: brandon, id: regular-italic, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: italic),
	(group: brandon, id: bold, font: ('Brandon Grot W01 Black', san-serif), weight: 700, style: normal),
	(group: brandon, id: bold-italic, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: italic),
	(group: clarendon, id: regular, font: ('Clarendon LT W01', serif), weight: 200, style: normal),
	(group: code, id: regular, font: (monospace), weight: 400, style: normal);

// Breakpoint Mixin
@mixin font($group, $id:regular){
	@each $font in $font-stack{
		@if($group == map-get($font, group) and $id == map-get($font, id)){
			font-family: map-get($font, font);
      		font-weight: map-get($font, weight);
      		font-style: map-get($font, style);
    	}
  	}
}

h1{
  @include font(brandon, light-italic);
}
p{
  @include font(brandon);
}
p i{
  @include font(brandon, regular-italic);
}
p b{
  @include font(brandon, bold);
}
blockquote{
  @include font(clarendon);
}
code{
  @include font(code);
}
```

### 3. Media Queries
简单的：
```scss
$tablet-width: 768px;
$desktop-width: 1024px;

@mixin tablet {
  @media (min-width: #{$tablet-width}) and (max-width: #{$desktop-width - 1px}) {
    @content;
  }
}

@mixin desktop {
  @media (min-width: #{$desktop-width}) {
    @content;
  }
}
```

MD风格的断点：
```scss
$_md-sm: 600px;
$_md-md: 960px;
$_md-lg: 1280px;

// x < 600
@mixin md-xs {
  @media (max-width: #{$_md-sm-width - 1px}) {
    @content;
  }
}

// 600 <= x < 960
@mixin md-sm {
  @media (min-width: #{$_md-sm-width}) and (max-width: #{$_md-md-width - 1px}) {
    @content;
  }
}

// 960 <= x < 1280
@mixin md-md {
  @media (min-width: #{$_md-md-width}) and (max-width: #{$_md-lg-width - 1px}) {
    @content;
  }
}
```