## DOM
### 节点
    表示整个文档的 document 对象在形式上也是一个 DOM 节点。 有 12 中节点类型  。实际上，我们通常用到的是其中的 4 个:
    1. document —— DOM 中的“入口点”。
    2. 元素节点 —— HTML 标签，树构建块。
    3. 文本节点 —— 包含文本。
    4. 注释 —— 有时我们可以将内容放入其中，它不会显示，但 JS 可以从 DOM 中 读取它。


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
          
         
          
