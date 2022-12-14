# C 语言运算符优先级（记忆口诀）

## 必背 
乘除余加减，移位再比较，等于不等于，位后是逻辑
> 乘除余，加减号，移位，比较，等于不等于，位与异或，位或，逻辑与，逻辑或

<table cellspacing="0" cellpadding="0" border="1">
<tbody>
<tr><td>优先级</td><td>运算符</td><td>名称或含义</td><td>使用形式</td><td>结合方向</td><td>说明</td></tr>
<tr><td rowspan="4">1</td><td>[]</td><td>数组下标</td><td>数组名[常量表达式]</td><td rowspan="4">左到右</td><td rowspan="4">零目运算符</td></tr>
<tr><td>()</td><td>圆括号</td><td>（表达式）/函数名(形参表)</td></tr>
<tr><td>.</td><td>成员选择（对象）</td><td>对象.成员名</td></tr>
<tr><td>-&gt;</td><td>成员选择（指针）</td><td>对象指针-&gt;成员名</td></tr>
<tr><td rowspan="9">2</td><td>-</td><td>负号运算符</td><td>-表达式</td><td rowspan="9">右到左</td><td rowspan="9">单目运算符</td></tr>
<tr><td>(类型)</td><td>强制类型转换</td><td>(数据类型)表达式</td></tr>
<tr><td>++</td><td>自增运算符</td><td>++变量名/变量名++</td></tr>
<tr><td>--</td><td>自减运算符</td><td>--变量名/变量名--</td></tr>
<tr><td>*</td><td>取值运算符</td><td>*指针变量</td></tr>
<tr><td>&amp;</td><td>取地址运算符</td><td>&amp;变量名</td></tr>
<tr><td>!</td><td>逻辑非运算符</td><td>!表达式</td></tr>
<tr><td>~</td><td>按位取反运算符</td><td>~表达式</td></tr>
<tr><td>sizeof</td><td>长度运算符</td><td>sizeof(表达式)</td></tr>
<tr><td rowspan="3">3</td><td>/</td><td>除</td><td>表达式/表达式</td><td rowspan="18">左到右</td><td rowspan="18">双目运算符</td></tr>
<tr><td>*</td><td>乘</td><td>表达式*表达式</td></tr>
<tr><td>%</td><td>余数（取模）</td><td>整型表达式/整型表达式</td></tr>
<tr><td rowspan="2">4</td><td>+</td><td>加</td><td>表达式+表达式</td></tr>
<tr><td>-</td><td>减</td><td>表达式-表达式</td></tr>
<tr><td rowspan="2">5</td><td>&lt;&lt;</td><td>左移</td><td>变量&lt;&lt;表达式</td></tr>
<tr><td>&gt;&gt;</td><td>右移</td><td>变量&gt;&gt;表达式</td></tr>
<tr><td rowspan="4">6</td><td>&gt;</td><td>大于</td><td>表达式&gt;表达式</td><</tr>
<tr><td>&gt;=</td><td>大于等于</td><td>表达式&gt;=表达式</td></tr>
<tr><td>&lt;</td><td>小于</td><td>表达式&lt;表达式</td></tr>
<tr><td>&lt;=</td><td>小于等于</td><td>表达式&lt;=表达式</td></tr>
<tr><td rowspan="2">7</td><td>==</td><td>等于</td><td>表达式==表达式</td></tr>
<tr><td>!=</td><td>不等于</td><td>表达式!=&nbsp;表达式</td></tr>
<tr><td>8</td><td>&amp;</td><td>按位与</td><td>表达式&amp;表达式</td></tr>
<tr><td>9</td><td>^</td><td>按位异或</td><td>表达式^表达式</td></tr>
<tr><td>10</td><td>|</td><td>按位或</td><td>表达式|表达式</td></tr>
<tr><td>11</td><td>&amp;&amp;</td><td>逻辑与</td><td>表达式&amp;&amp;表达式</td></tr>
<tr><td>12</td><td>||</td><td>逻辑或</td><td>表达式||表达式</td></tr>
<tr><td>13</td><td>?:</td><td>条件运算符</td><td>表达式1?&nbsp;表达式2:&nbsp;表达式3</td><td>右到左</td><td>三目运算符</td></tr>
<tr><td rowspan="11">14</td><td>=</td><td>赋值运算符</td><td>变量=表达式</td><td rowspan="11">右到左</td><td  rowspan="11">赋值运算符</td</tr>
<tr><td>/=</td><td>除后赋值</td><td>变量/=表达式</td></tr>
<tr><td>*=</td><td>乘后赋值</td><td>变量*=表达式</td></tr>
<tr><td>%=</td><td>取模后赋值</td><td>变量%=表达式</td></tr>
<tr><td>+=</td><td>加后赋值</td><td>变量+=表达式</td></tr>
<tr><td>-=</td><td>减后赋值</td><td>变量-=表达式</td></tr>
<tr><td>&lt;&lt;=</td><td>左移后赋值</td><td>变量&lt;&lt;=表达式</td></tr>
<tr><td>&gt;&gt;=</td><td>右移后赋值</td><td>变量&gt;&gt;=表达式</td></tr>
<tr><td>&amp;=</td><td>按位与后赋值</td><td>变量&amp;=表达式</td></tr>
<tr><td>^=</td><td>按位异或后赋值</td><td>变量^=表达式</td></tr>
<tr><td>|=</td><td>按位或后赋值</td><td>变量|=表达式</td></tr>
<tr><td>15</td><td>,</td><td>逗号运算符</td><td>表达式,表达式,…</td><td>左到右</td><td>从左向右顺序运算</td></tr>
</tbody>
</table>
> 说明：
同一优先级的运算符，运算次序由结合方向所决定。

## 大口诀：

单目、三目、赋值 右向左；

零目 第一

一目 第二

二目 第三（大三） 乘除余三,加减四

三目 第四

赋值逗号 第五

## 小口诀：

括号成员排第一;  //括号运算符\[\]() 成员运算符.  -> 

全体单目是第二;  //所有的单目运算符比如++、 --、 +(正)、 -(负) 、指针运算\*、&

乘除余三加减四;   //这个"余"是指取余运算即%

移位五，比较六;    //移位运算符：<< >> ，关系：> < >= <= 等

等不等排第七;    //即== 和!=

位与异或及位或;  "三分天下"八九十;   //这几个都是位运算: 位与(&)异或(^)位或(|)

与十一或十二;            //逻辑运算符:|| 和 &&            //注意顺序:优先级(&&)   高于 优先级(||)

三目还比赋值高,        //三目运算符优先级排到13 位只比赋值运算符和","高

逗号运算级最低!    //逗号运算符优先级最低