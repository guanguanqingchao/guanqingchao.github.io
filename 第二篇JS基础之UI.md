## DOM
### 查找节点
    表示整个文档的 document 对象在形式上也是一个 DOM 节点。 有 12 中节点类型  。实际上，我们通常用到的是其中的 4 个:
    1. document —— DOM 中的“入口点”。
    2. 元素节点 —— HTML 标签，树构建块。
    3. 文本节点 —— 包含文本。
    4. 注释 —— 有时我们可以将内容放入其中，它不会显示，但 JS 可以从 DOM 中 读取它。
    

![image](https://github.com/guanguanqingchao/guanqingchao.github.io/blob/master/DOM.jpeg)

    最上面的树节点可以直接通过 document 属性来使用:
    
    <html> = document.documentElement
    <body> = document.body
    <head> = document.head
    
    firstChild 和 lastChild 属性是访问第一个和最后一个子元素的快捷方式
    hasChildNodes()用于检测节点是否有子节点 
    父节点可以通过 parentNode 访问
    nextSibling previousSibling访问兄弟节点
    childNodes 集合提供了对所有子节点包括其中文本节点的访问。不是数组，集合可迭代， for...of循环，Array.from()转成真正数组
    
    修改自己节点不能通过childNodes[i]=...   只读，实时状态
    
    
    
    children —— 只获取类型为元素节点的子节点。
    firstElementChild ， lastElementChild —— 第一个和最后一个子元素。
    previousElementSibling ， nextElementSibling —— 兄弟元素。 parentElement —— 父元素。
    
    
    table元素支持 (除了上面给出的之外) 以下这些属性: 
        table.rows — 用于表示表中 <tr> 元素的集合。
        table.caption/tHead/tFoot用于访问元素 <caption> 、 <tfoot> 、 <tbody>
        table.tBodies — <tbody> 元素的集合(根据标准该元素数量可以很多)。
     
    <thead> 、 <tfoot> 、 <tbody> 元素提供了 rows 属性:
        tbody.rows — 表内部 <tr> 元素的集合。
     
    <tr> :
        tr.cells — 在给定 <tr> 元素下 <td> 和 <th> 单元格的集合。 
        tr.sectionRowIndex — 在封闭的 <thead>/<tbody> 中 <tr> 的编号。 
        tr.rowIndex — 在表中 <tr> 元素的编号。
        
    <td> 和 <th> :
        td.cellIndex — 在封闭的 <tr> 中单元格的编号。
    
    
    elem.querySelectorAll(css) 的调用将返回与给定 CSS 选择器匹配 elem 中的所有元素
    elem.querySelector(css) 第一个元素
    let elements = document.querySelectorAll('ul > li:last-child');
    
    elem.closest(css) 方法会查找与 CSS 选择器匹配的最接近的祖先。 elem 自己也会被搜索。
    elemA.contains(elemB)
    
    
### 节点属性：type、tag、 contents
    
    
  ![image](https://github.com/guanguanqingchao/guanqingchao.github.io/blob/master/event.jpeg)
    
    elem.constructor.name  查看DOM节点类型
    
#### “nodeType” 属性

    nodeType 属性提供了一个获取 DOM 节点类型的旧方法。
    它有一个数值:
    elem.nodeType == 1 是元素节点， 
    elem.nodeType == 3 是文本节点， 
    elem.nodeType == 9 是 document 对象，
    
#### 标签属性:nodeName 和 tagName

    tagName属性仅用于Element节点。 
    nodeName 是为任意 Node 定义的:
        对于元素，它的意义与 tagName 相同。 
        对其他节点类型(text、comment 等)，则是拥有一个字符串的节点类型
    elem.innerHTML+="something" 可以添加更多content，完全重写  仅元素节点
    outerHTML包含本身
    
    nodeValue/data:文本节点内容
    
    textContent:纯文本，去掉所有tag
    
    hidden elem.hidden = true; 隐藏元素     elem.hidden = !elem.hidden
 
#### 特性和属性 Attribute 译为“特性”，Property 译为“属性”

    特性 —— 写在 HTML 中。 
    属性 —— 是一个 DOM 对象。
    
    DOM属性
    HTML标签可能拥有特性，当浏览器读取 HTML 文本并根据标签生成DOM 对象，它会辨别标准化特性然后以此创建 DOM 属性。
    标准化特性：body的id   input的type
    
    如果一个特性不是标准化的，DOM 属性就不存在这个特性
    读取非标准化特性：
    elem.hasAttribute(name) —— 检验是否拥这个特性。 
    elem.getAttribute(name) —— 获取到这个特性值。 
    elem.setAttribute(name, value) —— 设置这个特性值。 
    elem.removeAttribute(name) —— 移除这个特性。

#### 非标准化的特性，dataset

    <div show-info="name"></div>
    <div show-info="age"></div>

 
        let user = {
            name: "Pete",
            age: 25
        };
        for (let div of document.querySelectorAll('[show-info]')) { // 找到特性
            let field = div.getAttribute('show-info');
            div.innerHTML = user[field]; // Pete，然后是年龄
        }
        
    所有以 “data-” 开头的特性值可以给编程人员正常使用，同时它还是 dataset 合法值。
    如果一个 elem  有一个键名是 "data-about" 的特性，那么可以通过 elem.dataset.about 取到这个合法值。
### 修改文档内容
    
    document.createElement(tag) 用给定的标签创建一个新元素:
    document.createTextNode(text) 用给定的文本创建一个文本节点
    
    parentElem.appendChild(node) 将 node 作为 parentElem 最后一个子元素。
    parentElem.insertBefore(node, nextSibling) 在 parentElem 的 nextSibling 前面插入 node 。
    parentElem.replaceChild(node, oldChild) 将 parentElem 的 oldChild 替换为 node 。
    parent.removeChild(node)
    
    
    在开头插入/在末尾插入/在前面插入/在后面插入   node节点
    This set of methods provides more flexible insertions:
    
    node.append(...nodes or strings) —— 在 node 最后一个孩子节点 后面插入节点或者字符串，
    node.prepend(...nodes or strings) —— 在 node 第一个孩子节点 前面插入节点或者字符串，
    node.before(...nodes or strings) —— 在 node 前面插入节点或者字符串，
    node.after(...nodes or strings) —— 在 node 后面插入节点或者字符串，
    node.replaceWith(...nodes or strings) —— 将 node 替换为节点或者字符串。
    node.remove() —— 移除 node 。
    
![image](https://github.com/guanguanqingchao/guanqingchao.github.io/blob/master/nodeinsert.jpeg)
    
    在相邻的 HTML 标签中插入/文本/元素  
    elem.insertAdjacentHTML(where,html)  Html标签
   
    beforebegin
    afterbegin 
    beforeend 
    afterend
    
    div.insertAdjacentHTML('beforebegin', '<p>Hello</p>');
    div.insertAdjacentHTML('afterend', '<p>Bye</p>');
    
![image](https://github.com/guanguanqingchao/guanqingchao.github.io/blob/master/option.jpeg)
    
    
     elem.cloneNode(true) 深克隆 false只克隆本身
     
### 样式和类
 
    elem.className  获取元素的class，动态设置class
    elem.classList 是一个特殊对象，它拥有 add/remove/toggle 的类方法。
        elem.classList.add/remove("class") —— 添加/移除类。
        elem.classList.toggle("class") —— 如果类存在就移除，否则添加。
        elem.classList.contains("class") —— 返回 true/false ，检查给 定类。
        可迭代  for of
    elem.style.backgroundColor     获取元素的内联style的样式，无法获取class
    elem.style.marginTop = "20px"  带单位
     // we can set special style flags like "important" here  可以设置多个样式属性
    table.style.cssText = `
        color: red !important;
        background-color: yellow; 
        width: 100px; 
        text-align: center;
    `;
     table.setAttribute('style', 'font-size:50px;color:yellow')
     
     获取样式 getComputedStyle(elem)
### 尺寸与滚动

![image](https://github.com/guanguanqingchao/guanqingchao.github.io/blob/master/scroll.jpeg)


    几何属性仅为显示出来的元素计算。
    如果元素(或其任何祖先)在文档中显示为 display:none 或本身不在文档中，则所有几何属性都是 0 或者值为 null  
    
    offsetParent, offsetLeft/Top 
        offsetParent 是最近的祖先元素:
            1. CSS 定位( position 为 absolute 、 relative 或 fixed )， 
            2. 或者 <td> 、 <th> 、 <table> ，
            3. 或者 <body> 。
            
        offsetLeft/offsetTop 提供相对于元素左上角的 x/y 坐标
        
    offsetWidth/Height 元素的“外部”宽度/高度。换句话说，它的完整大小包括边框border。
    clientTop/Left 元素内部边框，有滚动条的时候，border+滚动条的宽度
    clientWidth/Height 元素边框内区域的大小 不包含滚动条宽度 content+padding
    scrollWidth/Height clientWidth/Height+包括滚动(隐藏)部分
    scrollLeft/scrollTop 元素隐藏、滚动部分的宽度/高度。可改变。将 scrollTop 设置为 0 或 Infinity 将使元素分别滚动到顶部/底部。
    
     如果元素滚动到底，下面等式返回true，没有则返回false.
     element.scrollHeight - element.scrollTop === element.clientHeight
     其他参考：https://github.com/iuap-design/blog/issues/38
    
 ### Window 的尺寸和滚动
 
    文档可视范围的宽度高度 document.documentElement.clientWidth/clientHeight
         window.innerWidth; // 整个窗口的宽度
         document.documentElement.clientWidth; // 窗口减去滚动条的宽度  实际的窗口宽度
         
    整个文档的宽度高度，包括滚动区域外的部分
    let scrollHeight = Math.max(
        document.body.scrollHeight, document.documentElement.scrollHeight, document.body.offsetHeight,            document.documentElement.offsetHeight, document.body.clientHeight, document.documentElement.clientHeight
        );
        
    读取：当前窗口滚动 window.pageXOffset/pageYOffset 只读
    改变：当前窗口滚动 
        window.scrollBy(x,y)滚动页面至相对于现在位置的 (x, y) 位置 【相对当前位置定位】 
        window.scrollTo(pageX,pageY) 滚动页面至相对于文档的左上角的(x,y)位置，【绝对定位】就好像设置scrollLeft/scrollTop。
                        回到顶部, 我们可以用 scrollTo(0,0) 。
        elem.scrollIntoView(top) 滚动到正好elem可视的位置(elem与窗口的顶部/底部对齐)
                        如果 top=true (默认值)，页面滚动使 elem 会出现到窗口顶部。元素的上边 缘与窗口顶部对齐。
                        如果 top=false ，则页面滚动使 elem 会出现在窗口底部。元素的下边缘与窗 口底部对齐。
        
 ### 坐标
 
     窗口坐标：窗口的坐标是从窗口的左上角开始计算的，不会考虑文档滚动，基于窗口计算

             对应position:fixed
             elem.getBoundingClientRect()返回窗口坐标对象 
             窗口坐标用 clientY和clientX表示
     
     
     
     文档坐标：从文档的左上角开始计算，而不是窗口
             
             对应position:absolute
             页面没有滚动：窗口坐标和文档坐标一致
             页面有滚动：
                 pageY = clientY + 文档垂直部分滚动的高度。 
                 pageX = clientX + 文档水平部分滚动的宽度。
    

    

#### 练习

    https://plnkr.co/edit/RzDsKC5VqX7w6khH5Fxa?p=preview
    https://plnkr.co/edit/nYRyp8959wqIOBge8TnJ?p=preview
    https://plnkr.co/edit/xJmpkNCoe9MGDLAmPG9R?p=preview
    
## 事件

### 事件类型
- HTML事件 <input value="Click me" onclick="alert('Click!')" type="button">  大小写不敏感
- DOM2  elem.onclick = function(){} 不能添加多个事件处理器,覆盖 大小写敏感
- DOM3  elem.addEventListner('click',handler,false) false冒泡 true捕获
### 事件对象
- event.type 事件类型 'click'等
- event.currentTarget 与this相同，处理事件的元素，冒泡到的元素
- event.target 实际被点击的元素
- event.clientX / event.clientY 鼠标事件中光标相对于窗口的坐标。
### 冒泡和捕获

    DOM 事件标准描述了事件传播的 3 个阶段:
        1. 捕获阶段 —— 事件(从 Window)向下到达元素上。 
        2. 目标阶段 —— 事件到达目标元素。
        3. 冒泡阶段 —— 事件从元素上开始冒泡。
        
    禁止冒泡： event.stopPropagation() 
    
### 事件委托

 如果我们有许多元素是以类似的方式处理的，那么我们就不需要给每个元素分配一个处理器 —— 而是在它们共同的祖先上面添加一个处理器
 事件必须冒泡
 event.target 可以得到实际在哪里点击的
 事件委托一般分为两步：
    1. 我们向元素添加一个特殊属性。
    2. 用文档范围级的处理器追踪事件，如果事件发生在具有特定属性的元素上 —— 则 执行该操作。
    
    <button data-toggle-id="subscribe-mail"
        Show the subscription form
    </button>
    <form id="subscribe-mail" hidden> 
        Your mail: <input type="email">
    </form> 
    
    document.addEventListener('click', function(event) { 
        let id = event.target.dataset.toggleId;
        if (!id) return;
        let elem = document.getElementById(id);
        elem.hidden = !elem.hidden; 
        }
     );
     
     Counter: <input type="button" value="1" data-counter>
     One more counter: <input type="button" value="2" data-counter>

        document.addEventListener('click', function(event) {
        if (event.target.dataset.counter != undefined) { // if the attribute exists... 
            event.target.value++;
        }
        }); 
     

    
    




#### 练习
菜单切换显示隐藏 https://plnkr.co/edit/dJVvZgfE5M0t2JcpVs2X?p=preview
关闭按钮 https://plnkr.co/edit/L2538XJlIzpIcVlqoezg?p=preview
简易Carousel https://plnkr.co/edit/KIDLTiyDj4EZZtLSW05Q?p=preview
树形切换显示  https://plnkr.co/edit/qOUQDcMX3XBKIUoMshR2?p=preview
## 表单、控件

### 常见的表单以及表单属性

           <form name="basic-info">
              <fieldset>
                  <legend>Personalia:</legend>
                  Name: <input type="text"><br>
                  Email: <input type="text"><br>
                  Date of birth: <input type="text">
                  
                  <select id="select">
                      <option value="JS">JS</option>
                      <option value="CSS">CSS</option>
                      <option value="react">REACT</option>
                  </select>
              </fieldset>
          </form>

     


          <form name="info">
              <label> First name:</label>
              <input type="text" name="firstname" value="guan"><br>
              Last name: <input type="text" name="lastname" value="qingchao"><br>
              PassWord: <input type='password' name="pwd"><br>

              请选择你喜欢的城市
              <input type="radio" name="city" value="chengdu">ChengDu
              <input type="radio" name='city' value="hangzhou">HangZhou<br>
              请选择你喜欢的人物：<input type="checkbox" name="renwu" value='caocao'>CaoCao
              <input type="checkbox" name="renwu" value="GuanYu">GuanYu<br>
              <label>请输入你的观点：</label><br>
              <textarea rows="10" cols="40" placeholder="Please input your views">

              </textarea>
              <br>
              <br>
              <br>
              <input type="submit" value="Submit">
          </form>
          
          ****  可以通过document.form获取到form表单，也可以直接通过form的name来获取 ***
          document.forms.info.elements.lastname.value  //qingchao
          info.elements.firstname.value  //guan
          
          **** 反向引用 ***
          info.lastname   info.lastname.form
          
          
           *** select有options selectedIndex value等属性
           basicInfo.select.options[2].selected = true
           basicInfo.select.selectedIndex = 2;
           basicInfo.select.value = 'react'
           *** 从 multi-select 中获取所有选定的值
            let selected = Array.from(select.options)
                           .filter(option => option.selected) 
                           .map(option => option.value);
          
         
          
