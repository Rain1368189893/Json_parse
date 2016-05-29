# Json_parse
## javascript编写json解析器

>JSON有6种类型的值：对象、数组、字符串、数字、布尔值（true和false）和特殊值null。空白（空格符。制表符。回车符和换行符）可被插到任何值的前后。

###定义json_parse函数
######将函数赋值给一个实例对象json_parse,这样的好处是为实例对象创建了私有变量和特权方法。

####特权方法
* 1 error   //当某处发生错误时调用
* 2 next    //获取下一个字符
* 3 number  //解析一个数字值
* 4 string  //解析一个字符串值
* 5 white   //跳过空白
* 6 word    //true/false或null
* 7 array   //解析一个数组值
* 8 object  //解析一个对象值

####value方法

解析一个JSON值。它可以是对象、数组、字符串、数字或一个词。

###核心代码
<pre>
value = function (){
        //解析一个JSON值。它可以是对象、数组、字符串、数字或者一个词。
        white();
        console.log(ch);
        switch (ch) {
          case '{':
            return object();
          case '[':
            return array();
          case '"':
            return string();
          case '-':
            return number();
          default:
            return ch >= '0' && ch <= '9' ? number() : word();
        }
      };
      //返回json_parse函数。它能访问上述所有的函数和变量。
      return function (source, reviver) {
        var result;
        text = source;
        at = 0;
        ch = ' ';
        result = value();
        white();
        if(ch){
          error("Syntax error");
        }
        //如果存在reviver函数，就递归地对这个新结构调用walk函数，
        //开始时先创建一个临时的启动对象，并以一个空字符串作为键名保存结果，
        //然后传递每个“名/值”对给reviver函数去处理可能存在的转换。
        //如果没有reviver函数，我们就简单地返回这个结果。
        return typeof reviver === 'function' ?
          function walk(holder, key){
            var k, v, value = holder[key];
            if(value && typeof value === 'object'){
              for(k in value){
                if(Object.hasOwnProperty.call(value, k)){
                  v = walk(value, k);
                  if(v !== undefined){
                    value[k] = v;
                  }else{
                    delete value[k];
                  }
                }
              }
            }
            return reviver.call(holder, key, value);
          }({'':result},'') : result;
      };
}();
</pre>
###JSON解析测试
![JSON](http://i4.buimg.com/f2de885fdae3ceb3.png)
###参考
* 《JavaScript语言精粹》 P142-P147
* https://github.com/douglascrockford/JSON-js
