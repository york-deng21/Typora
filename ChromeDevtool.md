* ##### 通用功能

  * Copy:复制指定对象的内容
  * Store:暂存到全局(右键箭头)
  * Snippets:添加代码片段（ctrl + p，输入感叹号执行）
  * Command:(ctrl + shift + p)
    * cap：截屏（自选、全屏、节点）
    * show rendering：
      * paint flashing：界面的样式渲染
      * layer borders：界面渲染的层数
    * Animations：监听CSS动画并记录
    * changes：记录在浏览器中对CSS样式做的改动

* ##### 快捷符号

  * ###### 对ELEMENTS的操作
  
    * 回车：选中鼠标指定节点
    * H：隐藏/显示当前节点
    * F2：格式化节点代码
    * 右键、scroll into view：跳转到界面中对应节点
    * 右键、break on：断点（、属性改变、节点改变）
  
  * ###### 对console的操作：
  
    * $n：选择元素节点（0是当前点的元素，1、2、3依次为以前点击的）
    * $$(’选择器‘)：选择元素节点
    * $_：上一次的输出结果
    * bug:
      * 引用类型输出两次，第一次后改变数据，会导致第一个输出的的堆中数据发生改变，造成第一次的输出与堆中的值不一样，最好通过JSON.Stringify()进行两次输出
  
    * console.table/dir/group/count/info/error/warn/time
      * table:转换为表格console.table([{a:1,b:2},{a:3,b:5}])
      * dir:将对象转换为json类型
      * group：将包含的数据分组（以groupEnd结束）
      * count：监听执行次数
      * error：以报错的形式打印
      * warn：以警告的形式打印
      * time：调试包含代码的执行时间（以timeEnd结束）
    * async console
  
  * Sourse:
  
    * XHR：对请求加断点