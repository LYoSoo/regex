# 1.基本匹配

- 正则表达式是由一些字母和数字组成的一个搜索模版。  

  > ​	例如一个正则表达式 the,  表示匹配接连的三个字母 "the"。

```javascript
const reg = /the/g
const regResult = "The cat sat on the mat. the one".match(reg);
regResult // ["the", "the"];
```

> 正则表达式是区分大小写字母的，也就是字符串"The" 不会被匹配

# 2.元字符

- 正则表达式内置了一些特殊的模版，代表了相应的意思，也就是元字符。相当于给我们一些工具，我们借助 元字符 可以使我们的正则表达式更加简洁。

| 元字符  | 描述                                                         |
| :-----: | ------------------------------------------------------------ |
|    .    | 句号匹配任意单个字符 除了换行符。                            |
|   []    | 字符的种类，匹配方括号内的任意字符 例如： [0-9]              |
|   [^]   | 否定的字符种类，匹配除了方括号内部的任意字符  \[^0-9\]。除了0-9以外的字符 |
|    *    | 匹配 > = 0 个重复的在* 之前的字符  可以为0                   |
|    +    | 匹配 > = 1个重复在+ 之前的字符   不可以0  最少为1            |
|   ？    | 标记？之前的字符为可选                                       |
| ｛n,m｝ | 匹配num个大括号之前的字符或者字符集{ n < num < m};           |
|  {xyz}  | 匹配与xyz完全相等的字符串                                    |
|   \|    | “或" 运算符， 匹配符号前面或者后面的字符                     |
|    \    | 转义字符，用于匹配保留的字符 如： [ ] ( ) { } . *  + ? ^ $ \ \| |
|    ^    | 从开始行开始匹配                                             |
|    $    | 从末端开始匹配                                               |

## 2.1点运算符

- . 在运算符中是最简单的， 它匹配任意单个字符，但是不能匹配换行符。

  > ".ar" => The car parked in the garage, is 
  >
  > arrived.
  >
  > ```javascript
  > const reg = /.ar/g;
  > const regResult = "The car parked in the garage.".match(reg);
  > // regResult =>  ["car", "par", "gar"];
  > ```
  >
  > 

## 2.2 字符集

- 字符集就是用方括号来指定一个集合，在方括号中可以指定字符集的范围，字符集不关心里面的顺序

  > ​	"[Tt]he" 可以用来匹配 "the" "The";
  >
  > ```javascript
  > const reg = /[Tt]he/g;
  > const regResult = "The car parked in the garage.".match(reg);
  > // regResult => ["The", "the"];
  > ```
  >
  > ​	ar[.] 用来匹配 ar. 的字符串
  >
  > ```javascript
  > const reg = /ar[.]/g
  > const regResult = "park a car.".match(reg);
  > // regResult => ["ar."]
  > ```

### 2.21否定字符集

- ”^“ 一般时用来表示一个字符串的开头，但是当他用在方括号内的时候，表示这个字符集是否定的，例如 \[^c]ar 表示了匹配 后面跟着ar 的除了 c 的任意字符

  > ```javascript
  >   	>const reg = /[^c]ar/g;
  >   	>const regResult = "car bar garge.".match(reg);
  >   	>// regResult => ["bar", "gar"];
  >   	>```
  > ```

## 2.3 重复次数

> ​	上面表格提到了不同 * + ？ 三个元字符分别代表的含义。

### 2.3.1 * 号

> ​	*号匹配的是在 * 之前的字符串出现 大于或者等于 0 次。
>
> ​	a * 表示匹配 0 个或者多个以a 开头的字符
>
> ​	[a-z]* 表示匹配所有以小写字母开头的字符串
>
> ​    可以和表示匹配空格的 \s 连在一起使用
>
> ​    比如： \s\*cat\s\*   匹配的就是  cat 前面和后面均有一个或者多个空格
>
> ```javascript
> const reg = /\s*cat\s*/g;
> const regResult = "   cat   iscatff a".match(reg);
> // regResult => ["   cat  ", "cat"];
> ```
>
> 

### 2.3.2 +号

> - 号用来匹配 + 号之前的字符出现 > = 1次  
>
> c.+t      =>   "cafft cft"    ["cafft cft"]
>
> c[a]+t   =>  "caaaat cfat cat" => ["caaaat", "cat"]
>
> ```javascript
> const reg = /c[a]+t/g;
> const regResult = "caaat ct caft cat".match(reg);
> // regResult => ["caaat", "cat"]	此时无法匹配 ct
> ```
>
> 

### 2.3.3 ？号

> ​	正则表达式中 元字符 ？ 表示在符号前面的字符为可选，即可以出现 0 - 1 次
>
> ​	例如 [T]?he 可以匹配 字符 he 和 The
>
> ```javascript
> const reg = /[T]he?/g;
> const regResult = "The car is parked in the garage.";
> // regResult => ["The", "the"];
> ```
>
> 

## 2.4 ｛｝号

> ​	正则表达式中｛｝是一个量词，用来限定一个或者一组字符串可以重复出现的次数。
>
> ​	[0-9]{3,5} 表示匹配最少3位 最多5位的 0-9的数字。
>
> ```javascript
> const reg = /[0-9]{3,5}/g;
> const regResult = "12.1234 12.2 124.244 1234123".match(reg);
> // regResult => ["1234", "124", "244", "12341"];
> 
> ```
>
> ｛3｝则表示重复3次    {3, } 表示最少3次

## 2.5 （...） 特征标群

> ​	特征标群是一组写在 `(...)` 中的子模式。`(...)` 中包含的内容将会被看成一个整体，和数学中    小括号（ ）的作用相同。例如, 表达式 `(ab)*` 匹配连续出现 0 或更多个 `ab`。如果没有使用 `(...)` ，那么表达式 `ab*` 将匹配连续出现 0 或更多个 `b` 。再比如之前说的 `{}` 是用来表示前面一个字符出现指定次数。但如果在 `{}` 前加上特征标群 `(...)` 则表示整个标群内的字符重复 N 次。

- 我们可以在 ( ) 中用字符 | 表示 或

  ```javascript
  const reg = /(a|b|c)ar/g;
  const regResult = "aar bar car dar ccar".match(reg);
  // regResult => ["aar", "bar", "car", "car"]
  
  ```

## 2.6 | 运算符

> ​	表示或 用来判断条件
>
> ​	(T|t)he | car    用来匹配的是 ( T | t)he 或者  car 
>
> ```javascript
> const reg = /(T|t)he|car/g;
> const regResult = "The car is the one".match(reg);
> // regResult =>  ["The", "car", "the"];
> 
> ```
>
> 

## 2.7转码特殊字符

> ​	反斜线 \ 在表达式中用于转码紧跟其后的字符， 用于指定 `{ } [ ] / \ + * . $ ^ | ?` 这些特殊字符。如果想要匹配这些特殊字符则要在其前面加上反斜线 `\`。 
>
> ​	例如 `.`是用来匹配除换行符外所有字符的，如果想要匹配句子中的 `.` 则要写成 `\.` 以下这个例子 `\.?`是选择性匹配`.`
>
> ```javascript
> const reg = /(f|c|m)at\.?/g;
> const regResult = "The fat cat sat on the mat.".match(reg);
> // regResult => ["fat", "cat", "mat."]
> 
> ```
>
> 



