# 正则
* 在编译时新建正则表达式
    * var regex=/xyz/;
* 在运行时新建正则表达式
    * var regex=new RegExp(“xyz”);
* 正则对象的属性
    * ignoreCase 
    * global 
    * multiline 
    * lastIndex  g修饰符时下一次开始搜索的位置
    * source 正则表达式的字符串形式
    * sticky：粘连 y
         * g修饰符只要剩余位置中存在匹配就可，而y修饰符确保匹配必须从剩余的第一个位置开始
    * flags：ig
* regex.test(str)   //true false
    * 如果带有g,每一次test方法都从上一次结束的位置开始向后匹配
    * 通过lastIndex指定开始搜索的位置，必须有g
    * lastIndex只对同一个正则表达式有效
    * 正则模式是空串，匹配所有字符串
* regex.exec(str) 返回匹配结果
    * 正则表达式包含组匹配，返回的数组第一个成员是整个匹配成功的结果，后面的成员是圆括号对应的匹配成功的组
    * 包含g，exec搜索的位置从上一次匹配成功结束的位置开始
* 字符串对象的方法
    * str.match(regex)：所有匹配的子字符串 
        * lastIndex对match无效
        * 带有g 一次性返回所有匹配成功的结果
        * match的时候不要又用组匹配又用g，这样组匹配会被忽略
    * str.search(regex)：
        * 匹配开始的位置 总是从头开始,lastIndex无效
    * str.replace(regex，str)：替换后的字符串 加g匹配所有 
        * 消除字符串收尾两端的空格：str.replace(/^\s+|\s+$/g,””);
        * replace第二个参数$n
        * “hello world”.replace(/(\w+)\s(\w+)/,”$2$1”);  //world hello
        * $`匹配结果前面的文本 $’匹配结果后面的文本 
        * $&匹配的子字符串
            * “abc”.replace(‘b’,[$`-$&-$’])  //a[a-b-c]c
        * replace方法的第二个参数可以是函数
            * 将每个匹配的内容替换为函数返回值
            ```javascript
              var a = "sdFhyGioJl".replace(/[A-Z]/g,function(match){
                  return "-"+match.toLowerCase();
              });
              console.log(a); //sd-fhy-gio-jl
            ```
            * 替换函数可以接受多个参数，第一个是捕获到的内容，后面是捕获到的组匹配
            * 倒数第二个参数是捕获到的内容在整个字符串中的位置，最后一个参数原字符串
      * str.split(regex,返回数组的最大成员数)：返回一个数组，分割后的各个成员
          * “a, b,c, d”.split(/, */,2);   [a,b]
          * 如果带有组匹配，括号匹配的部分也会作为数组成员返回
              * “aaa*a*”.split(/(a*)/)  [“”,”aaa”,”*”,”a”,”*”]
* 元字符
    * . 匹配除回车、换行、行分隔、段分割以外的所有字符
    * ^ 字符串开始位置
    * $ 字符串结束位置
        * /^test$/ 从开始到结束只有test
    * | /11|22/  匹配11或22
    * 有特殊含义的字符，如果要匹配他们本身，要加上反斜杠
        * \+ \^ \. \[ \] \$ \( \) \| \* \+ \? \{ \} \\\
        * 如果使用new RegExp() 转义需要使用两个\\
    * \cX 相当于ctrl+[A-Z]
    * \b退格 \n 换行 \r 回车 \t制表 \v垂直制表 \f 换页 
    * \0 null \xhh 两位十六进制 \uhhhh 四位十六进制
    * [xyz]有一系列字符可供选择，只要匹配其中一个就好了
    * [^xyz]除了这个字符类中字符，其他字符都可以匹配
        * [^]匹配一切字符，包括换行符，比.好用
        * 脱字符只有在第一个位置才有意义，否则就是字面意义
        * [a-c][0-9]连字符 必须在[]中   - 必须在头尾两个字符之间才有意义
    * 预定义模式：
        * \d 相当于[0-9]  
        * \D 相当于[^0-9]
        * \w相当于[a-zA-Z0-9_] 任意字母数字下划线
        * \W相当于[^a-zA-Z0-9_]
        * \s相当于[\t\n\r\v\f]
        * \S相当于[^\t\n\r\v\f]
        * \b词的边界  
        * \B非词边界
            * /\bworld/.test(“hello-world”); //true
            * /\bworld/.test(“helloworld”); //false
        * [\S\s] 一切字符
    * 重复类 {n}重复几次 {n,}至少重复几次  
        * {n,m}不少于n次，不多于m次
        * 量词符 
            * ? {0,1} 
            * \* {0,} 
            * \+ {1，}
* 贪婪模式改为非贪婪模式 
    * 量词符后面加一个问号 +? *?
    * “aaa”.match(/a+?/); [“a”]
* 修饰符 g i m
    * /world$/.test(“world\n”); //false
    * /world$/m.test(“world\n”); //true
    * /^b/m.test(“a\nb”) //true
* 组匹配 test match
    * /(fred)+/.test(“fredfred”) true
    * “abcabc”.match(/(.)b(.)/); [abc,a,c]
    * /(.)b(.)\1b\2/.test(“abcabc”)  true
    * “hello world”.replace(/(\w+)\s(\w+)/,”$2$1”);
* 具名组匹配
    * (?<year>\d{4})
    * matchRes.groups.year
    * 解构赋值
    ```javascript
    let {groups: {one, two}} = /^(?<one>.*):(?<two>.*)$/u.exec('foo:bar');
    one //foo
    two //bar
    ```
* 非捕获组：（?:x）不返回该组匹配的内容
    * “abc”.match(/(?:.)b(.)/);  [abc,c]
* 先行断言：x(?=y) x只有在y前面才匹配，y不记入结果
    * “abc”.match(/b(?=c)/); [b]
* 先行否定断言：x(?!y) x只有不在y前面才匹配，y不记入结果
* 后行断言：(?<=y)x  x只有在y后面才匹配，y不计入结果
* 后行否定断言：(?<!y)x x只有不在y后面才匹配
*

