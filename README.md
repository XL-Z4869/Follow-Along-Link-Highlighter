# Follow-Along-Link-Highlighter
效果：https://xl-z4869.github.io/Follow-Along-Link-Highlighter/index.html

当鼠标移到a标签处时，为标签添加一个白色的背景框，同时，移动视口时，背景框也不会消失
### 一、知识点
#### element.getBoundingClientRect()
mdn https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect

返回元素的大小及其相对于视口的位置。返回值是DOMRect对象，包含left,right,top,bottom属性，除了width和height之外，其他属性都是相对于左上角位置而言
<table>
<thead>
<th>Attribute</th>
<th>Type</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>bottom</td>
<td>float</td>
<td>Y轴，相对于视口原点（viewport origin），矩形盒子的底部</td>
</tr>
<tr>
<td>height</td>
<td>float</td>
<td>矩形盒子的高度，相当于bottom-top</td>
</tr>
<tr>
<td>left</td>
<td>float</td>
<td>X轴，相对于视口原点（viewport origin），矩形盒子的左部</td>
</tr>
<tr>
<td>right</td>
<td>float</td>
<td>X轴，相对于视口原点（viewport origin），矩形盒子的右部</td>
</tr>
<tr>
<td>top</td>
<td>float</td>
<td>Y轴，相对于视口原点（viewport origin），矩形盒子的顶部</td>
</tr>
<tr>
<td>width</td>
<td>float</td>
<td>矩形盒子的宽度，相当于right-left</td>
</tr>
<tr>
<td>x</td>
<td>float</td>
<td>x轴横坐标，矩形盒子左边相对于视口原点的距离</td>
</tr>
<tr>
<td>y</td>
<td>float</td>
<td>轴横坐标，矩形盒子顶部相对于视口原点的距离</td>
</tr>
</tbody>
<table>

### 二、主要步骤
#### 1.元素使用
* 获取a标签
* 同时监听a元素
* 创建新元素，加样式
```
const lis_a = document.querySelectorAll('a')
const highlight = document.createElement('span')
highlight.classList.add('highlight')
document.body.appendChild(highlight)

lis_a.forEach(a => a.addEventListener('mouseenter', setPosition))
```
2.使用Element.getBoundingClientRect()方法
```
function setPosition() {
    const rectObject = this.getBoundingClientRect()
    console.log(rectObject);
    const rect = {
        width: rectObject.width,
        height: rectObject.height,
        top: rectObject.top + window.scrollY,
        left: rectObject.left + window.scrollX
    };
    console.log("rect");
    console.log(rect);
    // highligh.classList.add('highligh')
    highlight.style.width = `${rect.width}px`
    highlight.style.height = `${rect.height}px`
    highlight.style.transform = `translate(${rect.left}px,${rect.top}px)`
}
```
3.解决问题
发现视图缩小后，新加的元素不会随着a的位置变化而变化，因此需要定义一个新变量，让他记住当前的状态
```
let now=null
function enterHandler(){
	now=this;
setPosition()
}

lis_a.forEach(a => a.addEventListener('mouseenter', enterHandler))//监听amouseenter事件，记录当下的a，
window.addEventListener('resize', setPosition)  //监听window的resize事件
```
