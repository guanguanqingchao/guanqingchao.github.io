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
    用于访问元素 <caption> 、 table.tBodies — <tbody> 元素的集合(根据标准该元素数量可以很多)。
    <thead> 、 <tfoot> 、 <tbody> 元素提供了 rows 属性:
    tbody.rows — 表内部 <tr> 元素的集合。 <tr> :
    tr.cells — 在给定 <tr> 元素下 <td> 和 <th> 单元格的集合。 tr.sectionRowIndex — 在封闭的 <thead>/<tbody> 中 <tr> 的编号。 tr.rowIndex — 在表中 <tr> 元素的编号。
    <td> 和 <th> :
    td.cellIndex — 在封闭的 <tr> 中单元格的编号。
    
    
    elem.querySelectorAll(css) 的调用将返回与给定 CSS 选择器匹配 elem 中的所有元素
    elem.querySelector(css) 第一个元素
    let elements = document.querySelectorAll('ul > li:last-child');
    
    elem.closest(css) 方法会查找与 CSS 选择器匹配的最接近的祖先。 elem 自 己也会被搜索。
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
          
         
          
