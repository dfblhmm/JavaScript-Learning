# JavaScript

## 1 语言基础

### 1.1 ==三种变量声明方式==

>
>
>var

- 无块级作用域，为函数作用域
- 存在变量提升——把变量声明提升到==当前作用域==的最前面，不提升赋值操作
- 在全局作用域中声明的变量，会成为==window对象==的属性
- 可以重复声明

---

>
>
>let

- 块级作用域——由一对`{}`产生
- 不存在变量提升——暂时性死区
- 不能依赖==条件声明模式==，因为使用该关键字声明的变量会被限制在当前块级作用域

```html
<script>
  if (typeof age === 'undefined') {
    let age // age被限制在当前if{}中
  }
  age = 30; // 该赋值形同全局赋值
</script>

<script>
  console.log(age) // 30 age成为了全局变量
</script>
```

>

- 不允许在同一个块作用域中重复声明
- for循环中迭代变量声明仅限于当前for循环内部，不会泄露到循环块外部

---

>
>
>const

- 声明变量时必须对其==初始化==
- 声明变量后，不能修改该变量所指向的==内存地址==

---



### 1.2 ==数据类型==

>
>
>原始类型

- undefined —— 未定义

- null —— 空值(对==空对象==的引用)

- number —— 数值型，包括整数和浮点数

- string —— 字符串类型

- boolean —— 布尔类型

  - 其他数据类型与boolean类型之间的转换
  
| 数据类型  |    转换为true的值     | 转换为false的值 |
| :-------: | :-------------------: | :-------------: |
|  String   |      非空字符串       |  "" (空字符串)  |
|  Number   | 非零数值 (包括无穷值) |     0，NaN      |
|  Object   |       任意对象        |      null       |
| Undefined |     N/A (不存在)      |    undefined    |

- symbol —— 符号，用于确保对象属性使用唯一标识符，不会发生属性冲突的危险

---

>
>
>复杂类型

- Object —— 对象

  每个Object实例都有如下属性和方法

- `constructor`：用于创建当前对象的函数

- `isPrototypeof(object)`：用于判断当前对象是否为另一个对象的原型

- `hasOwnProperty(propertyName)`：用于判断当前对象实例（不是原型）上是否存在给定的属性。要检查的属性名必须是==字符串==

- `propertyIsEnumerable(propertyName)`：用于判断给定的属性是否可以使用for-in语句枚举

- `toLocaleString()`：返回对象的字符串表示，该字符串反映对象所在的本地化执行环境

- `toString()`：返回对象的字符串表示

```javascript
const time = new Date()
console.log(time.toString()) // Mon Mar 01 2021 14:04:23 GMT+0800 (中国标准时间)
console.log(time.toLocaleString()) // 2021/3/1下午2:04:23
```

- `valueOf()`：返回对象对应的字符串、数值或布尔值表示。打印对象时==默认调用==

---

>
>
>typeof 操作符

- 最适合用来判断一个变量是否是==原始类型==

| typeof 操作符输出 |      对应类型      |
| :---------------: | :----------------: |
|    "undefined"    |     undefined      |
|     "boolean"     |      boolean       |
|     "string"      |       string       |
|     "number"      |       number       |
|     "object"      | object 或 ==null== |
|    "function"     |        函数        |
|     "symbol"      |       symbol       |

---



### 1.3 语句

>
>
>标签语句

- 典型应用场景：嵌套循环

```javascript
label: for(let i = 0; i < 5; i++) {
  console.log(i)
  if (i === 3) continue label
}
```

---

>
>
>switch语句

- JS中switch语句可以用于==所有数据类型==

- 条件的值可以是常量或表达式
- switch语句在比较每个条件时会使用==全等操作符==，因此不会强制转换数据类型

---



### 1.4 变量、作用域与内存

#### 1.4.1 ==原始值与引用值==

- 原始值——最简单的数据

  - 保存原始值的变量是==按值==访问的，因为我们操作的就是存储在变量中的==实际值==
  - 原始值不能有属性，即使给原始值添加属性不会报错

  ```javascript
  let name = 'jack'
  name.age = 27
  console.log(name.age) // undefined
  ```
  - 通过变量把一个原始值赋值给另一个变量时，原始值会被复制到新变量的位置，并且==不会相互干扰==
  - 原始值大小固定，因此保存在==栈内存==上

- 引用值——由多个值构成的对象

  - 引用值是保存在内存中的对象，存储在==堆内存==上，JS不允许直接操作对象所在的==内存空间==
- 在操作对象时，实际上操作的是该==对象的引用==而非对象本身，因此保存引用值的变量是==按引用==访问的
  
- 把引用值从一个变量赋值给另一个变量时，实际上复制的是一个==指针==，它指向存储在==堆内存中的对象==，操作完成后，两个变量实际上会指向同一个对象，因此会==相互干扰==
  
- JS中所有函数的参数都是==按值传递==的

```javascript
function setName(obj) {
  obj.name = 'Nicholas'
  obj = {} // obj变为一个指向本地对象的指针，该本地对象在函数执行结束是就被销毁了
  obj.name = 'jack'
}
let person = {}
setName(person)
console.log(person) // Nicholas
```

---

>
>
>instanceof 操作符

用于判断变量是否是给定==引用类型==的实例

```javascript
const arr = [1, 2, 3]
console.log(arr instanceof Array)
const reg = /^abc$/
console.log(reg instanceof RegExp)
```

---

#### 1.4.2 垃圾回收

- 离开作用域的值会被自动标记为可回收，然后在垃圾回收期间被删除

- 标记清理（主流）：先给当前不使用的值加上标记，再回来回收他们的内存

- 引用计数（不推荐）：会出现==循环引用==问题

---



### 1.5 基本引用类型

#### 1.5.1 ==字符串操作方法==

- `length`属性：获取字符串==长度==

- `concat(...strings: string[])`：用于连接两个或多个字符串，返回一个新字符串

- `slice()` / `substring()` / `substr()`

  - 第一个参数表示子字符串的==开始位置==
  - 若没有第二个参数，则意味着提取到==字符串末尾==
  - 对于 `slice() `/ `substring()`，第二个参数表示子字符串==结束位置 (不包含)==

  - 对于``substr()``，第二个参数表示返回的子字符串的==长度==

- 字符串位置方法：`indexOf()` 和 `lastIndexOf()`
  - 第一个参数为要搜索的字符串
  - 第二个参数为可选，表示==开始搜索的位置==
  - indexOf() 从前往后查找，lastIndexOf() 从后往前查找

- 字符串包含方法(ES6)

  - `startsWith() `/ `endsWith()` ：字符串是否以目标字符串==开头/结尾==，第二个参数可选，表示开始搜索的位置

  - `inclues()`：是否包含目标字符串

- `trim()`：去除字符串前后的==空格==

- `repeat(num:number)`：将字符串复制(num)次，返回拼接所有副本后的结果

- ==迭代与解构==

  ```javascript
  const str = 'hello'
  for (const ch of str) {
    console.log(ch)
  }
  console.log(...str) // h e l l 0
  ```

- `toUpperCase()` / `toLowerCase()`：转大/小写

>

- 字符串模式匹配方法

  - `match()`
  - `search(Regexp)`：返回模式第一个匹配的位置索引，没找到返回-1

  - ==`replace()`==：字符串替换操作
    - 第一个参数可以是一个正则表达式或字符串
    - 第二个参数可以是一个字符串或函数

  ```javascript
  const str2 = 'async await interface class'
  console.log(str2.replace(/a/g, 'e')) // esync eweit interfece cless
  ```

---



#### 1.5.2 URL编码

>
>
>encodeURIComponent

>encodeURIComponent

- 作用：==参数直接拼接在URL后面，处理请求Url中的特殊字符，如&==

```javascript
const url = 'http://localhost:3000/top/playlist?limit=100&cat=R&B/Soul'
// 使用encodeURIComponent 对tag中的特殊字符编码
url = 'http://localhost:3000/top/playlist?limit=100&cat=' + encodeURIComponent('R&B/Soul')
// 编码结果
http://localhost:3000/top/playlist?limit=100&cat=R%26B%2FSoul
```

- ==特殊字符及其URL编码==

| 字符 |        特殊字符的含义        | URL编码 |
| :--: | :--------------------------: | :-----: |
|  +   |      URL 中+号表示空格       |   %2B   |
| 空格 | URL中的空格可以用+号或者编码 |   %20   |
|  /   |       分隔目录和子目录       |   %2F   |
|  ?   |    分隔实际的 URL 和参数     |   %3F   |
|  %   |         指定特殊字符         |   %25   |
|  #   |           表示书签           |   %23   |
|  &   |  URL 中指定的参数间的分隔符  |   %26   |
|  =   |      URL 中指定参数的值      |   %3D   |

```javascript
//正则 
const regex = /[+ /?%#&=]/
if (regex.test(tag)) {tag = encodeURIComponent(tag)}
```

- ==另外的解决办法==**将参数使用params对象来接收参数**

---

>
>
>decodeURIComponent

==解码方法==

---



## 2 集合引用类型

### 2.1 Object

- 访问对象属性
  - 方式1：`对象.属性`
  - 方式2：`对象[属性名的字符串形式]`
- 变量作为对象属性名

```javascript
{
  [key]: 属性值
}
```

---

>变量作为对象属性名





### 2.2 Array

#### 2.2.1 创建数组

- 使用Array构造函数

```javascript
const arr = new Array()
const arr2 = new Array(10) // 创建初始长度为10的数组
const arr3 = new Array('red', 'green', 'orange') // ['red','green','orange']
```

- `Array.from()` (ES6)：将类数组 (任何可迭代对象，或者有一个==length属性==和可索引元素的结构) 转换为数组实例

```javascript
// 字符串会被拆分为字符数组
console.log(Array.from('hello')) // ["h", "e", "l", "l", "o"]
// 对现有数组执行浅复制
const arr = [1,2,3]
const arr2 = Array.from(arr) // [1,2,3]
console.log(arr === arr2) // false
```

- `Array.of()` (ES6)：可以把一组参数转换为数组

```javascript
console.log(Array.of(1,2,3)) // [1,2,3]
```

- 使用数组字面量表示法

```javascript
const arr = [1,2,3]
```

- ==可以通过修改length属性可以从数组末尾删除元素或添加元素==

```javascript
const arr = [1,2,3]
arr.length = 2 // [1,2]
arr.length = 3 // [1, 2, empty]
```

---



#### 2.2.2 检测数组

- instanceof 操作符：用于在只有==一个网页==（只有一个全局执行上下文）的情况下

- `Array.isArray()`方法

---



#### 2.2.3 迭代器方法(ES6)

- `keys()`：返回数组==索引==的迭代器
- `values()`：返回数组==元素==的迭代器
- `entries()`：返回==索引/值对==的迭代器

```javascript
const arr = ['red', 'green', 'orange']
console.log(Array.from(arr.keys())) // [0,1,2,3]
console.log(Array.from(arr.values())) // ["red", "green", "orange"]
console.log(Array.from(arr.entries())) // [[0, 'red'], [1, 'green'], [2, 'orange']]
```

---



#### 2.2.4 ==栈方法==

- `push()`：接收==任意数量==的参数，添加到数组末尾，返回==新数组的长度==

- `pop()`：删除数组的最后一个元素，返回==被删除的元素==

---

#### 2.2.5 ==队列方法==

- `shift()`：删除数组的第一项，返回被删除的元素
- `unshift()`：接收==任意数量==的参数，添加到数组开头，返回==新数组的长度==

---



#### 2.2.6 排序方法

- `reverse()`：反转数组

- `sort(fun: Function)`

  - 默认`sort()`会按照升序重新排序数组元素，为此`sort()`方法会对每一项调用`toString()`方法，根据==比较字符串来决定顺序==，即使数组元素都是数值类型

  ```javascript
  const arr = [0, 1, 5, 8, 10]
  console.log(arr.sort()) // [0, 1, 10, 5, 8] 非正常顺序
  ```

  - 为解决上述问题，`sort()`方法可以接收一个**比较函数**
    - 比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，就返回==负值==；两个参数相等，则返回==0==；否则返回==正数==

  ```javascript
  const array = [0, 1, 5, 8, 10]
  function compare(a, b) {
    if (a < b) {
      return -1
    } else if (a > b) {
      return 1
    } else {
      return 0
    }
  }
  console.log(array.sort(compare)) // [0, 1, 5, 8, 10]
  ```

- `sort()`和`reverse()`均返回==调用它们的数组的引用==

---



#### 2.2.7 ==操作方法==

`splice()`：最强大的数组方法

- ==删除==：需要给`splice()`传入==2==个参数
  - 第一个参数 (**必选**)：开始删除的索引
  - 第二个参数 (**可选**)：删除的个数，不选则删除到尾

```javascript
const a = [1, 2, 3, 4]
console.log(a.splice(1)) // [2, 3, 4]
console.log(a) // [1]
```

- ==插入==：需要传入==3==个参数
  - 第一个参数：开始的位置
  - 第二个参数：==0==（要删除的元素个数）
  - 第三个参数（可以是任意个）：要插入的元素

```javascript
const b = [1, 2, 3, 4]
console.log(b.splice(1, 0, 8, 8)) // []
console.log(b) // [1, 8, 8, 2, 3, 4]
```

- ==替换==：需要传入==3==个参数
  - 第一个参数：开始的位置
  - 第二个参数：要删除(替换)的元素的个数
  - 第三个参数（可以是任意个）：要插入的元素

```javascript
const c = [1, 2, 3, 4]
console.log(c.splice(1, 2, 8, 8, 8)) // [2, 3]
console.log(c) // [1, 8, 8, 8, 4]
```

- ==`splice()`返回值==：返回一个数组，包含从数组中被删除的元素（没有删除元素则返回一个==空数组==）

---



#### 2.2.8 搜索和位置方法

- 严格相等：`indexOf()` / `lastIndexOf()` / `includes()`

- 断言函数：`find()` / `findIndex()`

  - `find()`返回第一个匹配的元素
  - `findIndex()`返回第一个匹配元素的索引

  ```javascript
  const people = [
    { name: 'jack', age: 20 },
    { name: 'john', age: 22 }
  ]
  console.log(people.find((value, index, array) => value.age > 20)) // {name: "john", age: 22}
  console.log(people.findIndex((value, index, array) => value.age > 20)) // 1
  ```

---



#### 2.2.9 ==迭代方法==

- `every()`：判断数组中的==每一项==是否都满足给定条件，满足则返回true，否则返回false

- `filter()`：根据给定条件==过滤==数组，返回过滤后的==新数组==
- `forEach()`：==遍历==数组，==无返回值==
- `some()`：对数组每一项都运行传入的函数，如果==有一项==满足条件，则返回true，同时==终止迭代==
- `map()`：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组

```javascript
const e = [1, 2, 3, 4, 5]
// every()
console.log(e.every(value => value > 1)) // false
console.log(e.every(value => value < 6)) // true
// filter()
console.log(e.filter(value => value % 2 === 0)) // [2, 4]
// some()
console.log(e.some(value => value % 3 === 0)) // true
// map()
console.log(e.map(value => value ** 2)) // [1, 4, 9, 16, 25]
```

---



#### 2.2.10 ==归并方法==

- `reduce((上一个归并值，当前项，当前项的索引，数组本身) => {}, 初始值(可选))`
  - 第一个参数函数的返回值会作为下一次调用时的==第一个参数==

```javascript
const m = [1, 2, 3, 4, 5]
const result = m.reduce((prev, value, index, arr) => {
  return prev + value
}, 10)
console.log(result) // 25 = 1+2+3+4+5+10
```

- `reduceRight()`：与`reduce()`类似，只是方向相反

---



### 2.3 Map(ES6)

- Map可以使用==任意==JavaScript数据类型作为键

- 创建映射

  - 创建一个空映射

  ```javascript
  const map = new Map()
  ```

  - 创建的同时初始化实例，传入一个==可迭代对象==，需要包含键/值对数组

  ```javascript
  const map = new Map([[0, 'red'], [1, 'green'], [2, 'blue']])
  ```

- API
  - `set(key, value)`：添加 / 修改 键值对，该方法返回==映射实例==，依次可以链式调用
  - `get(key)`：获取给定的键对应的值
  - `has(key)`：查询给定的键是否存在
  - `delete(key)`：删除键/值对，存在对应的键值对返回true
  - `clear()`：清空所有键/值对
  - `size属性`：获取映射中的键/值对数量

---



### 2.4 Set(ES6)

- Set集合会自动去除重复的元素

```javascript
// 简单的数组去重
const arr = [1, 1, 2, 2, 3, 3, 4]
console.log([...new Set(arr)])
```

- 创建集合

  - 创建一个空集合

  ```javascript
  const s = new Set()
  ```

  - 创建的同时初始化实例，传入一个==可迭代对象==，需要包含插入到集合实例中的元素

  ```javascript
  const s = new Set([1, 2, 3, 2])
  ```

- API

  - `add(value)`：向集合中添加元素，该方法返回集合实例，因此可以链式多次调用
  - `has(value)`：检测集合中是否含有给定的元素

  - `size属性`：获取集合中的元素数量

  - `delete()` / `clear()`

---



## 3 对象、类与面向对象编程

### 3.1 理解对象

#### 3.1.1 属性的类型

- 数据属性：包含一个保存数据值的位置，数据属性有4个特性描述它们的行为

  - `[[Configurable]]`：表示属性是否可以通过`delete`删除并重新定义，是否可以修改它的特性，是否可以把它改为访问器属性，默认为`true`
  - `[[Enumberable]]`：表示属性是否可以通过`for-in`遍历，默认为`true`
  - `[[Writable]]`：表示属性是否可以被修改，默认为`true`
  - `[[Value]]`：包含属性实际的值，默认为`undefined`

- 修改属性的默认特性——使用`Object.defineProperty()`方法

  - 第一个参数：要给其添加属性的对象
  - 第二个参数：属性的名称，==字符串==表示
  - 第三个参数：描述符对象，包含`configurable`、`enumerable`、`writable`、`value`
  - 在调用`Object.defineProperty()`时，`configurable`、`enumerable`、`writable`的值若不指定则会==默认设置为false==

  ```javascript
  const obj = {}
  Object.defineProperty(obj, 'name', {
    value: 'john'
  })
  delete obj.name
  console.log(obj) // {name: 'jhon'}
  obj.name = 'aaa'
  console.log(obj) // {name: 'jhon'}
  ```

- 访问器属性：不包括数据值，它们包含一个获取`(getter)`函数和`(setter)`函数

  - `[[Get]]`：获取函数，在读取属性时调用。默认值`undefined`
  - `[[Set]]`：设置函数，在写入属性时调用。默认值`undefined`
- 使用`Object.defineProperty`来定义访问器属性
  
- `Object.getOwnPropertyDescriptors(obj: Object)`：获取对象属性的特性

---



#### 3.1.2 合并对象

`Object.assign()`：ES6专门为合并对象提供的方法

- 将每个源对象中可枚举（`enumerable: true`）和自有（`Object.hasOwnProperty()`返回true）属性复制到目标对象上

- 第一个参数：目标对象
- 第二个参数：一个或多个源对象
- `Object.assign()`修改目标对象，同时也会返回==修改后的目标对象==
- `Object.assign()`实际上是对每个源对象执行的==浅复制==。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

>``

```javascript
const o = {
  name: 'jack',
  age: 20
}
const countryInfo = {
  country: {
    name: 'china'
  }
}
console.log(Object.assign(o, {gender: '男'}, country))
console.log(o) // {name: 'jack', age: 20, country: {name: '中国'}, gender: '男'}
countryInfo.country.name = '中国'
console.log(o) // {name: "中国"}
```

- 如果多个源对象有相同的属性，则使用==最后一个==复制的值

---



#### 3.1.3 ==增强的对象语法(ES6)==

- ==属性值简写==：属性名与变量名一样时可以简写，变量名会被自动解析为同名的属性名，变量的值作为属性值

```javascript
const name = 'jack'
const obj = { name }
console.log(obj) // { name: 'jack' }
```

- ==可计算属性==：使用变量来给属性名==动态赋值==

```javascript
const a = 'name'
const obj = {
  [a]: 'jack'
}
console.log(obj) // { name: 'jack' }
```

- ==方法的简写==

```javascript
const person = {
  _name: 'jack',
  get name() {
    console.log('调用了get方法')
    return this._name
  },
  set name(value) {
    console.log('调用了set方法')
    this._name = value
  }
}
```

---



#### 3.1.4 对象解构

```javascript
// 使用属性名作为变量名
const obj = {
  name: 'jack',
  age: 20
}
const { name, age } = obj
console.log(name, age) // 'jack' 20

// 使用自定义变量名
const { name: username, age: _age } = obj
console.log(username, _age) // 'jack' 20
```

- 如果引用的属性不存在，则该变量的值为`undefined`
- 在解构时可以使用默认值，适用于引用的属性不存在于源对象中时
- 解构复制实现的是浅拷贝
- 解构赋值连续写

```javascript
const obj = {
  person: {
    name: 'jack'
  }
}
const { person: { name } } = obj
// person 没有定义
console.log(name) // 'jack' 

const { person: { name: username } } = obj
console.log(username) // 'jack'
```

---



### 3.2 创建对象

#### 3.2.1 工厂模式

```javascript
function createObject(name, age) {
  const obj = new Object()
  obj.name = name
  obj.age = age
  obj.print = function() {
    console.log(this.name, this.age)
  }
  return obj
}
const obj = createObject('jack', 20)
```

---



#### 3.2.2 构造函数模式

```javascript
function Person(name, age) {
  this.name = name
  this.age = age
  this.print = function() {
    console.log(this.name, this.age)
  }
}
const person = new Person('jack', 20)
person.print() // 'jack' 20
```

- 构造函数与工厂模式的区别
  - 没有==显式==地创建对象
  - 属性和方法直接赋值给==this==
  - 没有==return==

- 使用`new`操作符创建对象实例会执行的操作

  - 在内存在创建一个对象
  - 这个新对象内部的`[[Prototype]]`特性被赋值为==构造函数的prototype属性==

  - 构造函数内部的this被赋值为这个新对象（即==this指向新对象==）
  - 执行构造函数内部的代码（给新对象添加属性）
  - 如果构造函数返回非空对象，则返回该构造函数对象；否则返回刚创建的新对象

- 创建出的实例对象的`constructor`属性指向构造函数本身

```javascript
console.log(person.constructor === Person) //true
```

- 构造函数的问题：其定义的方法会在每个实例上都创建一遍

```javascript
const person1 = new Person('jack', 20)
const person2 = new Person('mike', 22)
console.log(person1.print == person2.print) // false
```

---



#### 3.2.3 ==原型模式==

```javascript
function Student() {}
Student.prototype.name = 'jack'
Student.prototype.age = 20
Student.prototype.print = function() {
  console.log(this.name, this.age)
}
const s = new Student()
s.print() // 'jack' 20
```

- 使用原型模式定义的属性和方法是由所有实例共享的，因此不同实例访问到的属性和方法是==相同==的

```javascript
console.log(new Student().print === new Student().print) // true
```

>
>理解原型

---

>
>
>理解原型

- `prototype`：只要创建一个函数，就会为该函数添加一个==`prototype`属性（指向原型对象）==
- 默认情况下，所有原型对象`prototype`都会获得一个名为`constructor`属性，指回与之关联的构造函数

- `__proto__`：对象的原型，==所有对象均含有该属性==，指向==构造函数的`prototype`原型对象==

```javascript
function Person() {}
const p = new Person()
console.log(p.__proto__ === Person.prototype) // true
```

- ==正常的原型链都会终止于Object的原型对象，Object原型的原型是null==

```javascript
function People() {}
const people = new People()
console.log(people.__proto__ === People.prototype) // true
console.log(People.prototype.__proto__ === Object.prototype) // true
console.log(People.prototype.__proto__.constructor === Object) // true
console.log(Object.prototype.__proto__) // null
```

---



#### 3.2.3 对象迭代(ES8)

- `Object.values()`：返回对象==值==的==数组==
- `Object.entries()`：返回==键/值对的数组==

```javascript
const app = {
  name: 'jack',
  age: 20,
  gender: '男'
}
console.log(Object.values(app)) // ["jack", 20, "男"]
console.log(Object.entries(app)) // [["name", "jack"], ["age", 20], ["gender", "男"]]
```

---



### 3.3 继承

- 构造函数、原型、实例之间的关系：每个构造函数都有一个原型对象`prototype`，原型`prototype`有一个属性`constructor`指回构造函数，而实例内部有一个指针`__proto__`指向原型
- 原型链：一个函数的==原型==是另一个构造函数的==实例==，另一个原型也有一个指针指向另一个构造函数
- 通过原型链继承，实例既可以访问该构造函数==原型==上的属性和方法，也可以访问其他构造函数==实例==的属性和方法

```javascript
function A() {
  this.a = 'a'
}
function B() {}
B.prototype = new A()
B.prototype.constructor = B
B.prototype.b = 'b'

const b = new B()
console.log(b.a) // 'a'
console.log(b.b) // 'b'
```

- 原型与继承关系

  - 通过`instanceof`判断
  - 通过`构造函数.prototype.isPrototypeOf()`判断

  ```javascript
  console.log(b instanceof B) // true
  console.log(b instanceof A) // true 原型链继承
  console.log(B.prototype.isPrototypeOf(b)) // true
  console.log(A.prototype.isPrototypeOf(b)) // true 原型链继承
  ```

---



### 3.4 类(ES6)

#### 3.4.1 类定义

- 类定义与函数定义的区别

  - 函数声明可以提升，类定义不能
  - 函数受函数作用域限制，类受==块作用域==限制

  ```javascript
  {
    function Fun() {}
    class Person {}
  }
  console.log(Fun) // Fun() {}
  console.log(Person) // ReferenceError
  ```

- 类的构造：构造函数、实例方法、获取函数、设置函数和静态方法，这些都是非必须的

---



#### 3.4.2 类构造函数

- JS的类就是一种特殊函数
- 类标签符有`prototype`属性，而这个原型也有一个`constructor`属性指向类自身
- 可以使用`instanceof`操作符检查构造函数原型是否存在于实例的原型链中

---



#### 3.4.3 ==实例、原型和类成员==

>
>
>实例成员

- 通过`new`操作符创建
- 每个实例都对应一个==唯一==的成员对象，这意味着所有成员都不会在==原型上共享==

---

>
>
>原型方法

- ==在类块中定义的方法会作为**原型**方法==

- 添加到`this`的所有内容都会存在于==不同的实例上==，不能通过原型`prototype`来进行访问

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
  sayHi() {
    console.log(11, this.name)
  }
}

const person = new Person('jack')
person.sayHi() // 11 'jack'
Person.prototype.sayHi() // 11 undefined
```

- 在类方法中会开启==局部严格模式==

```javascript
class E {
  constructor(name) {
    this.name = name
  }
  say() {
    console.log(this)
  }
}
const e = new E('jack') 
e.say() // E {name: "jack"}
=========================
const ee = e.say
ee() // undefined
```

---

>
>
>访问器

- 类定义也支持获取和设置访问器。语法与行为和普通对象一样

```javascript
class Person {
  get name() {
    return this._name
  }
  set name(newName) {
    this._name = newName
  }
}
```

---

>
>
>静态成员

- 使用`static`关键字进行修饰，只能通过==`类名.属性/方法`==进行访问
- 静态成员中，this引用==类自身==

```javascript
class Student {
  static name = 'jay'
  static sayHi() {
    console.log(Student.name)
    console.log(this) // class Student {...}
  }
}
Student.sayHi() // 'jay'
```

---

>
>
>类成员

- 类中可以直接写==赋值语句==，相当于给每个实例对象添加了该属性

```javascript
class A {
  age = 18
}
const a = new A()
console.log(a.age) // 18
```



#### 3.4.4 类的继承

- 使用继承，类和原型上定义的方法都会被带到派生类
- this的值会反映调用相应方法的实例或者类
- ES6的继承可以使开发者方便地扩展内置类型

```javascript
class A {
  say() {
    console.log(this)
  }
  static say() {
    console.log(this)
  }
}
class B extends A {}
const a = new A()
const b = new B()
a.say() // A {}
A.say() // class A {...}
b.say() // B {}
B.say() // class B {...}
```

---

>
>
>super关键字

- `super`关键字只能在派生类中使用
- 在派生类构造函数中可以使用`super()`调用父类构造函数，相当于`super.constructor()`

- 在静态方法中可以使用`super`调用父类中的静态方法
- 在类构造函数中，不能在调用`super()`之前引用this

```javascript
class A {
  constructor(name) {
    this.name = name
  }
  static say() {
    console.log('static')
  }
  print() {
    console.log(this.name)
  }
}

class B extends A{
  constructor(name, age) {
    super(name)
    this.age = 20
  }
  static say() {
    super.say()
  }
  print() {
    console.log(this.name, this.age)
  }
}

const b = new B('jack')
b.print() // 'jack' 20
B.say() // 'static'
```

---



## 4 函数

- 每个函数都是`Function`类型的==实例==，而`Function`也有属性和方法，跟其他引用类型一样
- 函数名是指向函数对象的==指针==，因此一个函数可以有多个名称

- JS函数==没有重载==，因为没有函数签名。如果定义了同名函数，则后定义的函数会==覆盖==之前定义的函数



### 4.1 ==箭头函数==

- ==箭头函数不能使用`arguments`、`super`和`new.target`，也不能用作构造函数==
- ==箭头函数没有`prototype`属性==

>
>
>箭头函数this

- this指向：==最近作用域==中的this

- ==查找原则==：向==外层作用域==中，一层层查找this，直到有this的定义



### 4.2 默认参数(ES6)

- 默认参数不会反映到`arguments`上
- 默认参数值不限于原始值或对象类型，也可以使用==调用函数返回的值==
- 需要使用默认参数时，可以==显式传入undefined==

```javascript
function fun(name = 'jack') {
  console.log(name, arguments[0])
}
fun() // 'jack' undefined
fun('john') // 'john' 'john'

function fun2(name='jack', age) {
  console.log(name, age) 
}
fun2(undefined, 10) // 'jack' 10
```



### 4.3 参数扩展与收集

>
>
>扩展参数

```javascript
function sum() {
  let sum = 0
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i]
  }
  return sum
}
const arr = [1,2,3,4,5]
console.log(sum.apply(null, arr)) // 15
console.log(sum(...arr)) // 15
```

>

---

>
>
>参数收集

```javascript
const sum = (...arr) => {
  return arr.reduce((prev, current) => prev + current, 0)
}
console.log(sum(1,2,3,4,5)) // 15
```



### 4.4 函数声明与函数表达式区别

- JS引擎在执行任何代码之前，会先读取函数声明，并在执行上下文中生成函数定义

- 函数表达式必须等到代码执行到它那一行，才会在执行上下文中生成函数定义

```javascript
// 函数声明
fun() // 11
function fun() {
  console.log(11)
}
// 函数表达式
fun() // ReferenceError: Cannot access 'fun' before initialization
const fun = function() {
  console.log(11)
}
fun() // 11
```

---



### 4.5 ==函数内部this指向==

- 箭头函数内部`this`：引用==定义箭头函数的上下文==
- 标准函数内部`this`：把函数==作为方法调用==的上下文对象
- 内部函数无法直接访问外部函数中的`this`和`arguments`

```javascript
window.color = 'red'
const obj = {
  color: 'orange'
}
function sayColor() {
  console.log(this.color)
}
sayColor() // 'red'

obj.sayColor = sayColor
obj.sayColor() // 'orange'
```

---



### 4.6 ==函数属性和方法==

>
>
>函数属性

- 每个函数(==箭头函数除外==)都有两个属性

  - `length`：保存函数定义的==命名参数==的个数，剩余参数的数组不算在内
  - `prototype`：函数的原型对象


---

>
>
>函数方法

- `call()/apply()`：改变函数内部`this`指向，同时==调用函数==

  - 第一个参数指定函数内部的`this`
  - 第二个参数为传递的参数，区别：
    - `call()`中要将传递的参数进行展开，参数要==逐个传递==
    - `apply()`中可以使用`arguments`或者`Array`的实例进行参数传递

  ```javascript
  const a = [1,2,13,4,10]
  console.log(Math.max.call(Math, ...a)) // 13
  console.log(Math.max.apply(Math, a)) // 13
  ```

  ---

- `bind()`：创建一个新的函数实例，其`this`值会被==绑定==到传给`bind()`的对象

```javascript
window.age = 13
const o = {
  age: 20
}
function sayAge() {
  console.log(this.age)
}
sayAge() // 13 = window.age
const say = sayAge.bind(o)
say() // 20 = obj.age
```

---



### 4.7 ==闭包==

- 定义：引用了==另一个函数作用域中变量==的==函数==。通常是在嵌套函数中实现的
- ==闭包在被函数返回后，其作用域会一直保存在内存中，直到闭包被销毁==

- **缺点**
  - 闭包会保留它们包含函数的作用域，所以比其他函数更占用内存。过度使用闭包可能导致内存过度占用
  - 内存泄露。由于存在循环引用，内存不会被回收

```javascript
const obj = {
  name: 'tom',
  age: 18
}
function say() {
  const name = 'jack'
  console.log(this) // {name: "tom", age: 18}
  return function() {
    const age = 20
    console.log(this) // window {}
    return name + ' ' + age
  }
}
console.log(say.call(obj)())
```

---





## 5 期约与异步函数

### 5.1 期约基础

- 创建期约(`Promise`)的实例时需要传入==执行器==函数作为参数

- `Promise`的三种状态
  - 待定(`pending`)：期约的最初始状态
  - 解决(`fulfilled`)
  - 拒绝(`rejected`)

- 期约状态只要从`pending`转换为`fulfilled`或`rejected`后，期约的状态就不再改变
- 期约的状态不能被外部JavaScript代码修改
- 期约的`rejected`状态无法通过`try-catch`捕获，但可以抛出错误

>
>
>通过执行函数控制期约状态

- 执行器函数的两个职责
  - 初始化期约的异步行为
  - 控制状态的最终转换

```javascript
const p = new Promise((resolve, reject) => {
  resolve(1)
  reject() // 无效
})
```

---

- `Promise.resolve()`
  - 该静态方法可以把任何值转换为一个期约
  - 如果传入的参数本身是一个期约，则它的行为就相当于一个空包装。传入的期约的状态会决定该期约的最终状态
  - 任何==非期约值==都会被转换为==成功的期约==

- `Promise.reject()`：返回一个失败的Promise，传递的参数是什么，则返回为什么；==结果必然是失败==

---



### 5.2 ==期约的实例方法==

- `then()`、`catch()`、`finally()`处理程序均只能==异步执行==。处理程序会推进==消息队列==，因此在此之前需等待所有的==同步代码==执行完毕

>
>
>Promise.prototype.then()

- 该方法最多接收两个参数
  - 第一个参数：成功的处理函数
  - 第一个参数：错误的处理函数
  - 由于期约只能转换为最终状态一次，所以这两个操作一定是==互斥==的

- 该方法返回一个==新的期约实例==
- 返回错误值不会触发拒绝行为，而是会把错误对象包装在一个==解决的期约中==

```javascript
const p = new Promise((resolve, reject) => {
  resolve(11)
  // reject(22)
})
p.then(res => console.log(res), err => console.log(err)) // 11
```

>==Promise.then返回的新Promise的结果状态==

- ==返回的新的期约实例的状态==：由then()指定的==回调函数执行的结果==决定
  - 如果抛出异常，返回的新Promise对象状态变为==失败==，新对象的==err==的值为抛出的异常
  - 如果返回的是==非Promise对象==，新Promise对象的状态变为==成功==，新对象的==value==的值为返回的值

  - 返回的是另一个新的Promise对象，则该返回的Promise对象的结果就会成为then对象的Promise的结果

---

>
>
>Promise.prototype.catch()

- 该方法只接收一个参数：失败的处理程序

- 该方法实际是一个语法糖，相当于`Promise.prototype.then(null, () => {})`

- `Promise.prototype.catch()`方法返回一个==新的期约实例==

---

>
>
>Promise.prototype.finally()

- 不管期约转换为解决还是拒绝，该处理程序都会执行
- `Promise.prototype.finally()`方法返回一个==新的期约实例==，该新实例不同于`then()`或`catch()`返回的实例，多数情况下它都会原样后传父期约。无论父期约是解决还是拒绝，都会向后传递

```javascript
const p = new Promise((resolve, reject) => {
  // resolve()
  reject(11)
})
const pp = p.finally(() => console.log('期约状态发生了转换'))
console.log(pp) // Promise {<rejected>: 11}
```

---

>
>
>拒绝期约与拒绝错误处理

- 会成为拒绝的期约
  - `reject()`
  - `throw`
  - `Error()`
- 在期约中抛出错误时，因为错误实际上是从消息队列中==异步抛出==的，所以不会阻止运行时执行==同步代码==

---



### 5.3 期约连锁与合成

>
>
>期约连锁

- 由于每个期约的实例方法`then()`、`catch()`、`finally()`都会返回一个新的期约对象，这个期约对象又有自己的实例方法，因此可以==链式调用==

```javascript
const app = Promise.resolve(10)
app.then(res => console.log(res)) // 10
   .then(res => console.log(res)) // undefined
```

---

>
>
>期约合成

- `Promise.all()`

  - 该静态方法创建的期约会在一组期约全部解决之后再解决
  - 该方法接收一个==可迭代对象==作为参数，返回一个新期约
  - 如果==至少有一个==包含的期约==待定==，则合成的期约也会待定
  - 如果==有一个==包含的期约拒绝，则合成的期约也会拒绝。第一个拒绝的期约会将自己的拒绝理由作为==合成期约==拒绝的理由，之后再拒绝不会影响最终期约的拒绝理由
  - 只有==所有==期约都成功解决，合成期约才会成功，合成期约的解决值就是所有包含期约解决的==数组==，按照迭代器顺序

  ```javascript
  const a = Promise.all([
    Promise.resolve(10),
    Promise.resolve(22),
    // Promise.reject(18)
  ])
  a.then(res => console.log(res)) // [10, 22]
  a.catch(err => console.log(err)) // 18
  ```

---

- `Promise.race()`

  - 该静态方法返回一个包装期约，是一组集合中==最先解决或拒绝==的期约的镜像

  - 该方法接收一个==可迭代对象==作为参数，返回一个新期约

```javascript
const pp = Promise.race([
  Promise.resolve(10),
  Promise.reject(22)
])
pp.then(res => console.log(res), err => console.log(err)) // 10
```

---



### 5.4 中断Promise链

- 返回一个`Pending`状态的Promise实例

```javascript
Promise.resolve(1)
	.then(res => return new Promise())
```

---





### 5.5 异步函数(ES8)

>
>
>async

- `async`关键字用于声明异步函数。可以用在函数声明、函数表达式、箭头函数和==方法==上
- `async`关键字可以使函数具有异步特征，但总体上仍然是==同步==求值的
- 异步函数的返回值会被`Promise.resolve`包装成一个期约对象，异步函数始终返回期约对象
- 在异步函数中抛出错误会返回==拒绝==的期约
- 拒绝期约的错误不会被异步函数捕获

```javascript
async function foo() {
  Promise.reject(11)
}
foo().catch(err => console.log(err)) // Uncaught (in promise) 11
```

---

>
>
>await

- `await`关键字只能在==异步函数==中使用

- `await`关键字会暂停异步函数==后面==的代码，让出JavaScript运行时的执行线程，等待期约解决
- 对于拒绝的期约使用`await`会释放错误值（==将拒绝期约返回==），==后面的代码不会再执行==

```javascript
async function fun() {
  const p1 = await Promise.resolve(10)
  console.log(p1) // 10
  const p2 = await Promise.reject(20)
  console.log(p2) // Uncaught (in promise) 20
  console.log(30) // 这行代码不会执行
}
fun()
```

- `await`关键字并非只是等待一个值那么简单。JS运行时在碰到`await`关键字时，==会记录哪里暂停执行==。等到`await`右边的值可用了，JS运行时会向==消息队列==中推送一个任务，这个任务会==恢复==异步函数的执行

```javascript
async function foo() {
  console.log(2)
  await null
  console.log(4)
}
console.log(1)
foo()
console.log(3)
// 1
// 2
// 3
// 4
// 工作过程
//（1）打印1; (2)调用异步函数foo; (3)在foo中打印2; (4)在foo中的await暂停执行，为null值向消息队列中添加一个任务
// (5)foo()退出; (6)打印3; (7)同步线程的代码执行完毕; (8)JS运行时从消息队列中取出任务，恢复异步函数的执行
// (9)在foo()中恢复执行，await取得null值（这里并没有使用）; (10)在foo()中打印4; (11)foo()返回
```

- 如果`await`后面是一个期约，实际上会有==两个==任务被添加到消息队列中并被异步求值
- `await`只能拿到右边Promise==成功==的结果，对于错误需要使用`try-catch`捕获

```javascript
async function ajax() {
  try {
   let response = await fetch(url);
   let data = await response.json();
   console.log(data);
  } catch(e) {
    console.log("Oops, error", e);
  } 
}
```

---



### 5.6 ==Event Loop==

- ==调用栈==：执行同步任务，执行完毕后被弹出调用栈，==最先执行==
- ==消息队列==：用于存放异步任务(==宏任务==)，包括`fetch`、`setTimeOut`、`事件回调`等，==最后执行==
- ==微任务队列==：用于存放异步任务(==微任务==)，包括`Promise`、`async/await`创建的微任务

```javascript
const p = new Promise(resolve => {
  console.log(1)
  resolve(2)
})

function fun1() {
  console.log(3)
}

function fun2() {
  setTimeout(() => console.log(4), 0)
  fun1()
  console.log(5)
  p.then(res => console.log(res))
    .then(() => console.log(6))
}

fun2() // 1 3 5 2 6 4
```

---





## 6 BOM—浏览器对象模型

### 6.1 window对象

- `window`对象在浏览器中有两重身份：ECMAScript中的`Global`对象；浏览器窗口的JavaScript接口

---



### 6.2 location对象

- 提供了当前窗口中加载文档的信息，以及通常的导航功能

- 该对象既是`window`的属性，同时也是`document`的属性

|   属性   |                值                |                          说明                          |
| :------: | :------------------------------: | :----------------------------------------------------: |
|   hash   |           '"contents"            | URL散列值(#号后面跟零或多个字符)，如果没有则为空字符串 |
|   host   |        "www.baidu.com:80"        |                     主机名及端口号                     |
| hostname |         "www.baidu.com"          |                         主机名                         |
|   href   | "www.baidu.com:80/search/?id=10" |                 当前加载页面的完整URL                  |
| pathname |            "/search/"            |                  URL中的路径和文件名                   |
|   port   |              '"80"               |             请求的端口。没有则返回空字符串             |
| protocol |             "http:"              |                     页面使用的协议                     |
|  search  |             "?id=10"             |               URL的查询字符串。以"?"开头               |
|  origin  |         "www.baidu.com"          |                   URL的原地址。只读                    |

---



### 6.3 history对象

- 表示当前窗口首次使用以来用户的导航历史记录

>
>
>导航

- `go()`：可以在用户历史记录中沿任何方向导航，可以前进也可以后退。该方法只接收一个参数：
  - 整数：表示前进或后退多少步，正值代表前进，负值代表后退
  - 字符串：导航到历史中包含该字符串的第一个位置
- `forward()`：前进功能
- `back()`：后退功能
- `length`属性：表示历史记录中有多少条目

---

>
>
>历史状态管理

- `pushState()`：跳转页面，同时创建新的历史记录，会启用“后退”按钮
- `replaceState`：跳转页面，更新状态，不会创建新的历史记录，而是覆盖当前状态





## 7 DOM—文档对象模型

### 7.1.1 Node类型

>
>
>节点关系

- 每个节点都有一个`childNodes`属性，其中包含一个`NodeList`的实例。`NodeList`是一个==类数组==，用于存储可以按位置存取的有序节点。使用`previousSibling`和`nextSibling`可以在这个列表的节点间导航
- 每个节点都有一个`parentNode`属性，指向其`DOM`树中的父元素
- 父节点的`firstChild`属性指向`childNodes`中的第一个子节点；`lastChild`属性指向`childNodes`中的最后一个子节点

---

>

>
>
>==操纵节点==

- `appendChild()`：用于在`childNodes`列表==末尾==添加节点，返回==新添加的节点==。
  - 提示：==如果文档树中已经存在了 newchild，它将从文档树中删除，然后重新插入它的新位置==
- `insertBefore()`：将节点放入`childNodes`中的特点位置，该方法接收两个参数
  - 第一个参数：要插入的节点
  - 第二个参数：参照节点。节点会插入此参照节点之前。如果该参数为`null`，则节点插入列表末尾
- `replaceChild(要插入的节点, 要替换的节点)`：替换节点
- `removeChild(要移除的节点)`：移除节点；返回被移除的节点
- `cloneNode(b: boolean)`：复制节点。接收一个布尔值，true则为==深复制==
- ==追加元素==：`indertAdjacentHTML(position, element)`
  - 第一个参数：
    - `beforebegin`：元素自身的前面
    - `afterend`：元素自身的后面
    - `afterbegin`：插入元素内部的==第一个子节点之前==
    - `beforeend`：插入元素内部的==最后一个子节点之后==

---



### 7.1.2 Document类型

- 文档对象`document`是`HTMLDocument`的实例，是`window`对象的属性

>

- `document.documentElement`：指向`<html>`元素
- `document.body`：指向`<bode>`元素

>

- `title`属性：通过该属性可以读写页面的标题，修改后的标题也会反映在浏览器栏上

---

>
>
>定位元素

- `getElementById()`：根据`id`获取对应的元素
- `getElementsByTagName()`：根据==标签名==返回包含零个或多个元素的`NodeList`

- `getElementsByClassName()`(HTML5)：根据`class`返回包含元素的`NodeList`

---



### 7.1.3 ==Element类型==

>
>
>HTML元素

- 所有的**HTML**元素都通过`HTMLElement`类型表示，包括其直接实例和间接实例

- 所有**HTML**元素都有的标准属性：`id`、`title`、`className`

---

>
>
>属性操作

- `getAttribute(属性名)`：获取元素的属性值（包括==自定义属性==）
- `setAttribute(属性名, 属性值)`：设置属性（包括==自定义属性==）
- `removeAttribute(属性名)`：删除属性

---

>

- `document.createElement(创建元素的标签名)`：创建元素

- `childNodes`属性：包含元素的所有子节点

- `classList`属性(HTML5)上的方法

  - `add(className)`：向元素追加类名
  - `contains(className)`：元素是否包含给定的类名

  - `remove(className)`：移除元素的类名
  - `toggle(className)`：切换类名（有则删除；没有则添加）

---



## 8 事件

### 8.1 ==事件流==

>
>
>事件冒泡

- IE事件流被称为**事件冒泡**，事件被认定为从最==具体==的元素开始触发，然后向上传播至文档(`Document`)

```html
<!DOCTYPE html>
<html> 
<head>
  <title>Document</title>
</head>
<body>
	<div id="my">Click Me</div>  
</body>
</html>
```

- 在点击了页面中的`<div>`元素后，`click`事件会以如下顺序发生：

​      (1)`<div>`；(2)`<body>`； (3)`<html>`； (4)`document`

---

>
>
>事件捕获(几乎不用)

- 最==不具体==的节点最先收到事件，而最具体的节点应该最后收到事件

```html
<!DOCTYPE html>
<html> 
<head>
  <title>Document</title>
</head>
<body>
	<div id="my">Click Me</div>  
</body>
</html>
```

- 在点击了页面中的`<div>`元素后，`click`事件会以如下顺序发生：

​      (1)`document`；(2)`<html>`； (3)`<body>`； (4)`div`

---

>
>
>==DOM事件流==

- DOM2 Events规范规定事件流分为3个阶段：==事件捕获==、==到达目标==、==事件冒泡==
- 所有现代浏览器都支持`DOM事件流`，只有`IE8`及更早版本不支持

---



### 8.2 事件处理程序

>
>
>HTML事件处理程序

```html
<button onclick="console.log(11)">按钮</button>
<div onclick="sayHi()"></div>
<script>
  function sayHi(event) {
    alert('Hello')
    console.log(event.type, event.target)
  }	
</script>
```

---

>
>
>DOM事件处理程序

- `DOM0`事件处理程序

```html
<button id="my">按钮</button>
<script>
  const btn = document.getElementById('my')
  btn.onclick = function() {
    console.log('按钮被点击')
  }
</script>
```

- `DOM2`事件处理程序：`addEventListener()`和`removeEventListener()`
  - 这两个方法接收接收3个参数：事件名、事件处理函数和一个布尔值
  - 第三个参数布尔值为`true`则表示在**捕获阶段**调用事件处理程序；为`false`(默认值)则表示在**冒泡阶段**调用事件处理程序

---



### 8.3 事件类型

- `mouseenter/mouseleave`：用户把鼠标光标从元素外/内部移到元素内/外部时触发。这个事件==不冒泡==，也不会在光标经过后代元素时触发
- `mousewheel`：用户在使用鼠标滚轮时触发。mousewheel事件的`event`对象有一个属性`wheelDelta`，当鼠标滚轮向==前==滚动时，`wheelDelta`的值每次都是==120==；鼠标滚轮向==后==滚动时，`wheelDelta`的值每次都是==-120==
- 拖放事件
  - `dragstart`：在按住鼠标不放并开始移动鼠标的那一刻，被拖动元素上会触发该事件
  - `drag`：`dragstart`事件触发后，只要目标还被拖动就会持续触发`drag`事件
  - `dragend`：当拖放停止时，会触发该事件
- **移动端开发注意事件**
  - 不支持`dblclick`。双击时浏览器窗口会方法
  - 单指点击屏幕上的可点击元素会触发`mousemove`事件。如果操作则不会再触发其他事件；如果屏幕上没有变化，则会相继触发`mousedown`、`mouseup`、`click`事件。点触不可点击元素不会触发事件
  - `mousemove`事件也会触发`mouseover`和`mouseout`事件
  - 双指点触屏幕并滑动导致页面滚动会触发`mousewheel`和`scroll`事件

---





## 9 ==JSON==

>
>
>语法

- JSON本身可以是一个数值、对象或数组

- JSON语法支持3种类型的值
  - **简单值**：字符串、数组、布尔值和`null`可以出现。特殊值`undefined`、`symbol`不能在JSON中出现
  - **对象**：每个值可以是简单值，也可以是复杂类型
  - **数组**：数组的值可以是任意类型
- JSON没有变量、函数或对象实例的概念。JSON的所有记号都只为表示==结构化数据==
- JavaScript字符串与JSON字符串的主要区别：==JSON字符串必须使用**双引号**==

---

>
>
>JSON.stringify()

- 作用：将JavaScript序列化为JSON字符串

- 在序列化时，所有==函数==和==原型成员==都会被忽略，此外值为`undefined`的任何属性也会被跳过。最终得到的就是所有实例属性均为JSON数据类型的表示

- 该方法除了要序列化的对象，还可以接收2个可选参数

  - 第一个参数为过滤器，可以是数组或函数

    - 为数组：`JSON.stringify()`方法只会返回==该数组列出的对象属性==

    ```javascript
    const book = {
      title: 'Professional JavaScript',
      authors: ['Nicholas C.Zakas', 'Matt Frisbie'],
      edition: 4,
      year: 2017
    }
    // {"title":"ProfessionalJavaScript","edition":4}
    console.log(JSON.stringify(book, ['title', 'edition'])) 
    ```

    - 为函数：该函数接收2个参数—属性名(key)和该属性对应的属性值(value)。该函数返回的值就是序列化的结果

    ```javascript
    const book = {
      title: 'Professional JavaScript',
      authors: ['Nicholas C.Zakas', 'Matt Frisbie'],
      edition: 4,
      year: 2017
    }
    const text = JSON.stringify(book, (key, value) => {
      switch(key) {
        case 'authors':
          return value.join(',')
        case 'year': 
          return 2021
        case 'edition': // 该属性会被忽略
          return undefined
        default:
          return value
      }
    })
    // {"title":"Professional JavaScript","authors":"Nicholas C.Zakas,Matt Frisbie","year":2021}
    console.log(text)
    ```

  - 第二个参数控制==缩进==和==空格==

    - 为==数值==：表示每一级==缩进的空格数==
    - 为==字符串==：JSON字符串会使用该字符串来==代替空格==进行缩进

---

>
>
>JSON.parse()

- 作用：将JSON解析为原生JavaScript值

- 该方法可以接收一个额外的参数（还原函数），该函数接收属性名(key)和对应的属性值(value)
- 如果还原函数返回`undefined`，则结果中就会==删除相应的键==

```javascript
const json = {
  name: 'jack',
  age: 16,
  colors: ['red', 'blue']
}
const jsonText = JSON.stringify(json)

const js = JSON.parse(jsonText, (key, value) => {
  if (key === 'colors') {
    return value.includes('orange') ? value : [...value, 'orange']
  }
  return value
})
console.log(js) // {name: 'jack',age:16,colors:['red','blue','orange']}
```

---



## 10 网络请求与远程资源

### 10.1 XMLHttpRequest对象

- 使用XHR对象首先要调用`open()`方法，该参数接收3个参数
  - 请求类型：`'get'`、`'post'`等
  - 请求URL
  - 表示请求是否异步的布尔值
- 要发送定义好的请求，可以调用`send()`方法。如果不需要发送请求体，则必须传入参数`null`
- 监听XHR对象的`load`事件，说明已经收到请求
- 收到响应后，XHR对象的以下属性会被填充上数据
  - ==responseText==：作为响应体返回的文本
  - ==status==：响应的HTTP状态码
  - statusText：响应的HTTP状态码的描述
- 在收到响应之前如果想取消异步请求，可以调用XHR对象的`abort()`方法

---



### 10.2 进度事件

>
>
>load事件

- `load`事件在响应接收完成后立即触发，这样就不用检查`readyState`属性

---

>
>
>==progress事件==

- 该事件在浏览器接收数据期间，会反复触发
- 每次触发时，`progress`事件处理程序都会收到`event`对象，其`target`属性是XHR对象，且包含3个属性
  - `lengthComputable`：布尔值，表示进度信息是否可用
  - `position`：接收到的字节数
  - `totalSize`：响应头部定义的==总字节数==
- 为了保证正确执行，必须在调用`open()`方法之前添加`progress`事件的事件处理程序

---



### 10.3 跨域资源共享(CORS)

- ==IE10以下不支持CORS==

>
>
>预检请求

- `CORS`通过一种**预检请求**的服务器验证机制，允许使用自定义头部、除GET和POST之外方法，以及不同请求体内容类型
- `Access-Control-Allow-Origin`：决定响应请求，包含相同的源；如果资源公开，则包含为`'*'`
- `Access-Control-Allow-Methods`：==允许的方法==

- `Access-Control-Allow-Headers`：服务器允许的头部

---

>
>
>凭据请求

- 默认情况下，跨域请求不提供凭据（cookie、HTTP认证和客户端SSL证书）

- 可以通过`withCredentials`属性设置`true`来表明请求会发送凭据

- 如果服务器允许带凭据的请求，那么可以在响应中包含如下HTTP头部

  `Access-Control-Allow-Credentials: true`

---



### 10.4 JSONP

- JSONP格式包括两个部分：回调和数据
  - 回调是在页面接收到响应之后应该调用的函数，通常回调函数的名称是通过请求来动态指定的
  - 数据就是作为参数传给回调函数的JSON数据
- JSONP的调用就是通过==动态创建`<script>`元素并为`src`属性指定跨域URL实现==

```javascript
function handleResponse(response) {
  console.log(response)
}
const srcipt = document.createElement('script')
script.src = 'http://example.com?callback=handleResponse'
document.body.insertBefore(script, document.body.firstChild)
```

- 缺点
  - 它==只支持GET请求==而不支持POST等其它类型的HTTP请求
  - 它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。
  - jsonp在调用失败的时候不会返回各种==HTTP状态码==。
  - 安全性。万一假如提供jsonp的服务存在页面注入漏洞，即它返回的javascript的内容被人控制的。那么结果是什么？所有调用这个 jsonp的网站都会存在漏洞。

---



### 10.5 Fetch API

- `XMLHttpRequest`可以选择异步，而`Fetch API`必须是异步的

#### 10.5.1 基本用法

- `fetch()`是暴露在全局作用域的，包括主页面执行线程、模块和工作者线程

>
>
>分派请求

- `fetch()`只有一个必需的参数—获取资源的==URL==
- 请求完成、资源可用时，期约会解决为一个`Response`对象。这个对象是API的封装，可以通过它取得相应资源

---

>
>
>读取响应

- `text()`：获取响应的纯文本格式的内容，该方法返回一个==期约==，会解决为取得资源的完整内容
- `json()`：将获取的响应JSON化——相当于`JSON.parse()`

```javascript
fetch('http://localhost:3000/banner')
  .then(response => response.text())
  .then(res => console.log(JSON.parse(res)))

fetch('http://localhost:3000/banner')
  .then(response => response.json())
  .then(res => console.log(res))
```

---

>
>
>处理状态码和请求失败

- 通过`Response`对象的`status`(状态码)和`statusText`(状态文本)属性检查响应状态。成功响应通常返回==**200**==的状态码

```javascript
fetch('http://localhost:3000/banner')
  .then(({ status, statusText }) => console.log(status, statusText))
```

- 违反`CORS`、无网络连接、HTTPS错配及其他浏览器/网络策略问题都会导致==期约被拒绝==

---



#### 10.5.2 常用Fetch请求模式

>
>
>发送JSON数据

```javascript
const payload = JSON.stringify({
  foo: 'bar'
})
const headers = new Headers({
  'Content-Type': 'application/json'
})
fetch('/send-me-json', {
  method: 'POST',
  body: payload,
  headers
})
```

---

>
>
>在请求体中发送参数

```javascript
const payload = 'name=jack&age=20'
const headers = new Headers({
  'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
})
fetch('/send-me-json', {
  method: 'POST',
  body: payload,
  headers
})
```

---

>
>
>发送文件

```javascript
const imageFormData = new FormData()
const imageInput = document.querySelector('input[type="file"]')
imageFormData.append('image', imageInput.files[0])
fetch('/img-upload', {
  method: 'POST',
  body: imageFormData
})
```

---

>
>
>中断请求

Fetch API 支持通过`AbortController/AbortSignal`对中断请求。调用`AbortController.abort()`会中断所有网络传输。中断进行中的`fetch()`请求会导致包含==错误的拒绝==

```javascript
const abortController = new AbortController()
fetch('wikipedia.zip', { signal: abortController.signal })
	.catch(() => console.log('aborted!'))
// 100毫秒后中断请求
setTimeout(() => abortController.abort(), 100)
```

---



### 10.6 ==HTTP状态码==

| 状态码  |         状态码英文名称          |                           中文描述                           |
| :-----: | :-----------------------------: | :----------------------------------------------------------: |
|   100   |            Continue             |                   继续。客户端应继续其请求                   |
|   101   |       Switching Protocols       | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|         |                                 |                                                              |
| ==200== |               OK                |             ==请求成功。一般用于GET与POST请求==              |
|   201   |             Created             |               已创建。成功请求并创建了新的资源               |
|   202   |            Accepted             |              已接受。已经接受请求，但未处理完成              |
|   203   |  Non-Authoritative Information  | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
|   204   |           No Content            | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
|   205   |          Reset Content          | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
|   206   |         Partial Content         |            部分内容。服务器成功处理了部分GET请求             |
|         |                                 |                                                              |
|   300   |        Multiple Choices         | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| ==301== |        Moved Permanently        | ==永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替== |
|   302   |              Found              | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
|   303   |            See Other            |        查看其它地址。与301类似。使用GET和POST请求查看        |
|   304   |          Not Modified           | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
|   305   |            Use Proxy            |            使用代理。所请求的资源必须通过代理访问            |
|   306   |             Unused              |                    已经被废弃的HTTP状态码                    |
|   307   |       Temporary Redirect        |           临时重定向。与302类似。使用GET请求重定向           |
|         |                                 |                                                              |
| ==400== |           Bad Request           |           ==客户端请求的语法错误，服务器无法理解==           |
| ==401== |          Unauthorized           |                  ==请求要求用户的身份认证==                  |
|   402   |        Payment Required         |                        保留，将来使用                        |
| ==403== |            Forbidden            |      ==服务器理解请求客户端的请求，但是拒绝执行此请求==      |
| ==404== |            Not Found            | ==服务器无法根据客户端的请求找到资源（网页）==。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
|   405   |       Method Not Allowed        |                   客户端请求中的方法被禁止                   |
|   406   |         Not Acceptable          |          服务器无法根据客户端请求的内容特性完成请求          |
|   407   |  Proxy Authentication Required  | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
|   408   |        Request Time-out         |           服务器等待客户端发送的请求时间过长，超时           |
|   409   |            Conflict             | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
|   410   |              Gone               | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
|   411   |         Length Required         |    服务器无法处理客户端发送的不带Content-Length的请求信息    |
|   412   |       Precondition Failed       |                 客户端请求信息的先决条件错误                 |
|   413   |    Request Entity Too Large     | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
|   414   |      Request-URI Too Large      |        请求的URI过长（URI通常为网址），服务器无法处理        |
|   415   |     Unsupported Media Type      |               服务器无法处理请求附带的媒体格式               |
|   416   | Requested range not satisfiable |                     客户端请求的范围无效                     |
|   417   |       Expectation Failed        |               服务器无法满足Expect的请求头信息               |
|         |                                 |                                                              |
| ==500== |      Internal Server Error      |               ==服务器内部错误，无法完成请求==               |
| ==501== |         Not Implemented         |           ==服务器不支持请求的功能，无法完成请求==           |
| ==502== |           Bad Gateway           | ==作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应== |
|   503   |       Service Unavailable       | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
|   504   |        Gateway Time-out         |      充当网关或代理的服务器，未及时从远端服务器获取请求      |
|   505   |   HTTP Version not supported    |         服务器不支持请求的HTTP协议的版本，无法完成处         |

---



## 11 客户端存储

### 11.1 cookie

>
>
>限制

- cookie是与==特定域==进行绑定的。设置cookie后，它会与请求一起发送到==创建它的域==。这个限制可以保证cookie中存储的信息只对被认可的接受者开放，不被其他域访问
- cookie存储在==客户端磁盘上==，为保证它不会被恶意利用，浏览器会施加限制

---

>
>
>cookie的构成

- **名称**：唯一标识cookie的名称。cookie==不区分大小写==，cookie名必须经过==URL编码==
- **值**：存储在cookie里的字符串值。这个值必须经过==URL编码==
- **域**：cookie有效的域。发送到这个域的所有请求都会包含对应的cookie，这个值可能包含子域
- **路径**：请求URL包包含这个路径才会把cookie发送到服务器
- **过期时间**：表示何时删除cookie的==时间戳==。默认情况下，浏览器回话结束后会删除所有cookie。可以设置删除cookie的时间，这个值是==GMT==格式
- **安全标志**：设置之后，只在使用SSL安全连接(==HTTPS连接==)的情况下才会把cookie发送到服务器

---

>
>
>JavaScript中的cookie

- `doucment.cookie`：BOM中处理cookie的接口
- 所有名和值都是URL编码的，因此必须使用`decodeURIComponent()`编码

- `document.cookie`设置新的cookie字符串，该字符串被解析后会添加到原有的cookie中，该操作==不会覆盖==之前任何存在的cookie，除非设置了已有cookie
- 设置cookie时，只有cookie的==名称和值==是必需的

```javascript
document.cookie="name=jack"
```

---



### 11.2 Web Storage

>
>
>Storage类型

- `Storage`类型用于保存名/值对数据，直至存储空间上限(由浏览器决定)
- `Storage`类型的实例（`sessionStorage`和`localStorage`）增加了以下方法
  - `clear()`：删除所有键/值对
  - `getItem(name)`：获取给定name对应的值
  - `key(index)`：取得给定数值位置的名称
  - `removeItem(name)`：删除给定name的键/值对
  - `setItem(name,value)`：设置键/值对

---

>
>
>sessionStorage对象

- `sessionStorage`对象值存储==会话数据==，这意味着只存储到==浏览器关闭==
- 存储在`sessionStorage`中的数据==不受页面刷新影响==，可以在浏览器崩溃并重启后恢复
- 存储在`sessionStorage`中的数据只能由==最初存储数据的页面==使用

---

>
>
>localStorage对象

- 存储在`localStorage`中的数据会保留到==通过JS删除或者用户清除浏览器缓存==，存储在==本地磁盘中==
- `localStorage`中的数据不受页面刷新影响，也不会因关闭窗口、标签页或重启浏览器而丢失

---



## 12 模块

### 12.1 CommonJS

- **CommonJS** 使用`require()`指定依赖，使用`exports`对象定义自己的公共API

```javascript
const moduleA = require('./ModuleB')
module.exports = {
  say: moduleA.say()
}
```

---



### 12.2 ES6模块

- 带有`type="module"`属性的`<script>`标签会告诉浏览器相关代码应该作为==模块==执行，而不是传统脚本执行
- ==每个模块都有自己对应的作用域，不会相互影响==

>
>
>模块导出

>模板导出

- 方式一：在定义变量的时候加上==export==关键字

```javascript
export const flag = false;
export const sum = (num1, num2) => num1 + num2;
```

- 方式二：使用==export==关键字导出一个==包含导出变量的对象==

```javascript
const num = 20;
const dev = (num1, num2) => num1 - num2;
export {num, dev}
```

- ==默认导出==：`export default`——该导出方式只能导出==一个变量(函数，对象，类等)==，并且只能有一次==默认导出==

```javascript
//ModuleA.js
const address = '湖北';
export default address;	//只能导出一个变量

import add from "./ModuleA.js"  //add可以自定义
console.log(add); //湖北
```

```javascript
export default class Demo {
  
}
```

---

>
>
>模块导入

- 使用==import==关键字配合==对象解构==完成导入

```javascript
import {flag, sum, num, dev} from "./ModuleA.js";
//变量可以使用 as 起别名
import {flag as flag_img, sum, num as number, dev} from "./ModuleA.js";
```

- 统一==全部==导入

```javascript
import * as require from "./ModuleA.js" 	//require是别名对象，包含了导出模块中的所有变量和函数等
//使用
console.log(require.num);
```

- 直接导入并==执行==模块代码

```javascript
import './ModuleA.js';
```

- ==动态加载==

```javascript
const login = () => import('./login.vue')
```

---

>
>
>export default

- 该导出方式只能导出==一个变量(函数，对象等)==，并且只能有一次==默认导出==

```javascript

```

```javascript

```

---

