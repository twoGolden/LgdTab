## 基础用法
> template

	<lgd-tab :tabValue="tabValue" :tabIndex="tabIndex" />

> script

	export default {
		data() {
			return {
				tabValue: ['示例1',  '示例12'],
				tabIndex: 0
			}
		}
	}

## 参数说明
| 参数名 | 说明 | 默认 | 是否必填 | 参数类型 |
|:-----|---|-----|:-----:|:-----:|
|tabValue|tab列表||是| Array |
|tabIndex|tab选中下标|0|否| Number |
|textColor|文字颜色|#333|否|String|
|underlineColor|滑块颜色|#34b2fa|否|String|
|fontSize|字体大小|16|否|Number|
|background|组件背景颜色|#fff|否|String|

## 事件
| 事件名 | 说明 | 回调参数 |
|:-----|-------|:-----:|
|getIndex|获取当前点击下标值|tabIndex|

## 方法
| 方法名 | 说明 | 传入参数 |
|:-----|-------|:-----:|
|clickTab|选中传入的下标的tab|tabIndex|

## 隐藏滚动条

若组件出现滚动条，在App.vue样式中引入css

	<style>
		@import url("./components/lgd-tab/lgd-tab.css");
	</style>