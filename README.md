# webCourses

## Javascript
### Tips
- 预解析
- \+ 存在隐藏的强制类型转换
- string 基本包装类型:把简单数据类型包装成为了复杂数据类型
- null 是 js 中的一个小 bug,虽然是简单数据类型 但是用 typeof 可以得到返回的是一个空的对象 object

### 堆栈空间区别
- 栈（操作系统）:由操作系统自动分配释放存储函数的参数值、局部变量的值等。其操作方式类似于数据结构中的栈；
  - **简单数据类型存放到栈里面** string,number,boolean,undefined,null
- 堆（操作系统）:存储复杂类型（对象），一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收。

  - **复杂数据类型存放到堆里面** Object Array Date

- 简单数据类型是存放在栈里面 里面直接开辟一个空间存放的就是值
  - 简单数据类型传参的时候是直接传值
- 复杂数据类型 首先在栈里面存放地址 十六进制表示 然后这个地址指向堆中的数据
  - 复杂数据类型传参的时候实际是把变量在栈空间里保存的堆的地址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象

### DOM和BOM
- DOM Document Object Manage
- BOM Browser Object Manage

#### 表单属性Tips
- 表单input的值为value
- type="text" type="password"
- 隐藏元素 display:none
- 显示元素 display:block
- `<input type="checkbox" />` checkbox表单有checked属性 True为勾选 False为不勾选

#### 事件类型
- 注意循环事件绑定和事件触发的顺序
  - 循环事件绑定全部完成后，才进入到事件触发的过程，因此触发事件时，循环计数器次数=总次数。
  - eg: Day2 06-tab栏切换
- 获得焦点事件 onfocus
- 失去焦点事件 onblur
- 鼠标经过事件 onmouseover
- 鼠标离开事件 onmouseout

#### 自定义属性的操作
1.获取属性值
- element.属性
  - element.id
  - element.className
  - element.classList
- element.getAttribute('属性')
2.区别
- element.属性 获取内置属性值(元素本身自带的属性) 如果是自定义属性 则为undefined
- element.getAttribute('属性');主要获取自定义的属性(标准) 我们程序员自定义的属性
3.设置元素属性值
- element.属性 = '值' 设置内置属性值。
  - div.className = 'footer'
- element.setAttribute('属性','值') 主要针对于自定义属性
  - div.setAttribute('class', 'footer'); // class 特殊  这里面写的就是class 不是className
4.移除属性 removeAttribute('属性')
div.className="" 和div.removeAttribute('class')有一定区别
- div.className="" 还是会存在class `<div class></div>`
- div.removeAttribute('class') 不会存在class `<div></div>`
5.h5自定义属性
- h5新增的获取自定义属性的方法 它只能获取data-开头的
- 而元素.dataset 是一个集合里面存放了所有以data-开头的自定义属性
  - div.dataset {"index":2,"time":20}
  - div.dataset['index'] = 2
  - div.dataset.time = 20
- 如果自定义属性里面有多个-链接的单词，我们获取的时候采取驼峰命名法
  - `<div data-list-name="andy"></div>`
  - div.dataset.listName="andy"

#### 节点
##### 节点概述
一般地，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。
- 元素节点  nodeType  为 1
- 属性节点  nodeType  为 2
- 文本节点  nodeType  为 3 （文本节点包含文字、空格、换行等）
我们在实际开发中，节点操作主要操作的是元素节点

##### 节点层级
- 父节点 node.parentNode 得到的是离元素最近的父级节点(亲爸爸) 如果找不到父节点就返回为 null
  - var son = document.querySelector('.navs');
  - console.log(son.parentNode);
- 子节点 
  - parentNode.childNodes 返回包含指定节点的子节点的集合，该集合为即时更新的集合。
    - 注意：返回值里面包含了所有的子节点，包括元素节点，**文本节点**等。
    - 如果只想要获得里面的元素节点，则需要专门处理（判断nodeType==1）。 所以我们一般不提倡使用childNodes
    ```html
    var ul = document. querySelector(‘ul’);
    for(var i = 0; i < ul.childNodes.length;i++) {
      if (ul.childNodes[i].nodeType == 1) {
        // ul.childNodes[i] 是元素节点
        console.log(ul.childNodes[i]);
      }
    }
    ```
  - parentNode.children 是一个只读属性，返回**所有的子元素节点**。它**只返回子元素节点**，其余节点不返回
- 第一个和最后一个子节点
  - ol.children[0];
  - ol.children[ol.children.length - 1];
- 兄弟节点
  - nextElementSibling 返回当前元素下一个兄弟元素节点，找不到则返回null。 
  - previousElementSibling 返回当前元素上一个兄弟节点，找不到则返回null。
  - 但是以上两种有兼容性问题
  ```html
  function getNextElementSibling(element) {
      var el = element;
      while (el = el.nextSibling) {
        if (el.nodeType === 1) {
            return el;
        }
      }
      return null;
    }  
  ```
- 创建节点 document.createElement('tagName')
   - document.createElement() 方法创建由 tagName 指定的 HTML 元素。因为这些元素原先不存在，是根据我们的需求动态生成的，所以我们也称为动态创建元素节点。
- 添加节点
  - node.appendChild(child)
    - node.appendChild() 方法将一个节点添加到指定父节点的子节点列表末尾。类似于 CSS 里面的 after 伪元素。
  - node.insertBefore(child, 指定元素)
    - node.insertBefore() 方法将一个节点添加到父节点的指定子节点前面。类似于 CSS 里面的 before 伪元素。
- 删除节点 node.removeChild(child)
  - node.removeChild(child) 从 DOM 中删除一个子节点,返回删除的节点
  - 可以结合 node.children\[i\]进行删除元素
  - node.removeChild(node.children\[i\])
- 阻止链接跳转需要添加`javascript:void(0)`; 或者`javascript:;`
- 复制节点
  - node.cloneNode() 返回调用该方法的节点的一个副本
  - node.cloneNode(); 括号为空或者里面是 false 浅拷贝 只复制标签不复制里面的内容
  - node.cloneNode(true); 括号为 true 深拷贝 复制标签复制里面的内容
- 三种创建元素的方式
  - 1. document.write() 是直接将内容写入页面的内容流，但是**文档流执行完毕，则它会导致页面全部重绘**。只剩下write的内容
  - 2. innerHTML 创建元素
    - 将内容写入某个DOM节点，不会导致页面全部重绘
    - 创建多个元素效率更高(不要拼接字符串，采取数组形式拼接)，结构稍微复杂
  - 3. document.createElement(tagname) 创建元素
    - 创建多个元素效率稍低一点点，但是结构更清晰

#### DOM重点核心
关于dom操作，我们主要针对于元素的操作。主要有创建、增、删、改、查、属性操作、事件操作。
- 增
  - 1. appendChild
  - 2. insertBefore
- 删
  - 1. removeChild
- 改
  - 主要修改dom的元素属性，dom元素的内容、属性, 表单的值等
  - 修改元素属性： src、href、title等
  - 修改普通元素内容： innerHTML 、innerText
  - 修改表单元素： value、type、disabled等
  - 修改元素样式： style、className
- 查
  - 主要获取查询dom的元素
  - DOM提供的API getElementById、getElementsByTagName 古老用法 不太推荐 
  - H5提供的新方法 querySelector、querySelectorAll 提倡
  - 利用节点操作获取元素： 父(parentNode)、子(children)、兄(previousElementSibling、nextElementSibling) 提倡
- 属性操作
  - 主要针对于自定义属性。
  - 1. setAttribute：设置dom的属性值
  - 2. getAttribute：得到dom的属性值
  - 3. removeAttribute移除属性
- 事件操作
  - 给元素注册事件,采取事件源.事件类型 = 事件处理程序

#### 事件高级
##### 注册事件（绑定事件）
给元素添加事件，称为注册事件或者绑定事件。
注册事件有两种方式：传统方式和方法监听注册方式
**1. 传统注册方式**
- 利用 on 开头的事件 onclick 
- `<button onclick=“alert('hi~')”></button>`
- `btn.onclick = function() {} `
- 特点： 注册事件的唯一性
- 同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数
**2. 事件侦听注册事件** 
- `eventTarget.addEventListener(type, listener[, useCapture])` 方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。
  - type：事件类型字符串，比如 click 、mouseover ，注意这里不要带 on
  - listener：事件处理函数，事件发生时，会调用该监听函数
  - useCapture：可选参数，是一个布尔值，默认是 false。true 处于捕获阶段 document -> html -> body -> father -> son，false 处于冒泡阶段 son -> father ->body -> html -> document
- w3c 标准 推荐方式
- IE9 之前的 IE 不支持此方法，可使用 attachEvent() 代替
- 特点：同一个元素同一个事件可以注册多个监听器
- 按注册顺序依次执行
##### 删除事件（绑定事件）
**1. 传统注册方式**
`eventTarget.onclick = null;`
**2. 事件侦听注册事件** 
`eventTarget.removeEventListener(type, listener[, useCapture]);`
##### 事件对象
```javascript
eventTarget.onclick = function(event){};
eventTarget.addEventListener('click', fn);
function fn(e){}
// 这个 event 就是事件对象，我们还喜欢的写成 e 或者 evt
// event 对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态。
```
- e.target 和 this 的区别：
  - this 是事件绑定的元素， 这个函数的调用者（绑定这个事件的元素） 比如ul
  - e.target 是事件触发的元素 比如li

##### 阻止默认行为（事件）
```javascript
阻止默认行为（事件） 让链接不跳转 或者让提交按钮不提交
var a = document.querySelector("a");
a.addEventListener("click", function (e) {
  e.preventDefault(); //  dom 标准写法
});
```

##### 阻止冒泡
事件冒泡：开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点。
事件冒泡本身的特性，会带来的坏处，也会带来的好处，需要我们灵活掌握。
- 标准写法：利用事件对象里面的 stopPropagation()方法
`e.stopPropagation()`
- 非标准写法：IE 6-8  利用事件对象 cancelBubble 属性
`e.cancelBubble = true;`

##### 常见的鼠标事件
1. 禁止鼠标右键菜单
contextmenu主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单
```javascript
document.addEventListener('contextmenu', function(e) {
e.preventDefault();
})
```
2. 禁止鼠标选中（selectstart 开始选中）
```javascript
document.addEventListener('selectstart', function(e) {
e.preventDefault();
})
```
3. 鼠标对象事件
- client 鼠标在可视区的x和y坐标
  - e.clientX
  - e.clientY
- page 鼠标在页面文档的x和y坐标
  - e.pageX
  - e.pageY
- screen 鼠标在电脑屏幕的x和y坐标
  - e.screenX
  - e.screenY














