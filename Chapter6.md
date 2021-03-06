# Object

Object 是 ECMAScript 中最常用的类型之一

虽然 Object 的实例没有多少功能，但很适合存储和在应用程序间交换数据

显式地创建 Object 的实例有两种方式。第一种是使用 new 操作符和 Object 构造函数

```js
let person = new Object();
person.name = "Nicholas";
person.age = 29;
```

另一种方式是使用对象字面量（object literal）表示法

```js
let person = {
  name: "Nicholas",
  age: 29,
};
```

左大括号（{）表示对象字面量开始，因为它出现在一个表达式上下文（expression context）中。在 ECMAScript 中，表达式上下文指的是期待返回值的上下文。赋值操作符表示后面要期待一个值，因此左大括号表示一个表达式的开始。同样是左大括号，如果出现在语句上下文（statement context）中，比如 if 语句的条件后面，则表示一个语句块的开始

在对象字面量表示法中，属性名可以是字符串或数值，数值属性会自动转换为字符串

```js
let person = {
  name: "Nicholas",
  age: 29,
  5: true,
};
```

当然也可以用对象字面量表示法来定义一个只有默认属性和方法的对象，只要使用一对大括号，中间留空就行了

```js
let person = {}; // 与new Object()相同
person.name = "Nicholas";
person.age = 29;
```

对象字面量表示法通常只在为了让属性一目了然时才使用

> 在使用对象字面量表示法定义对象时，并不会实际调用 Object 构造函数

实际上开发者更倾向于使用对象字面量表示法。这是因为对象字面量代码更少，看起来也更有封装所有相关数据的感觉。事实上，对象字面量已经成为给函数传递大量可选参数的主要方式

虽然属性一般是通过点语法来存取的，这也是面向对象语言的惯例，但也可以使用中括号来存取属性。在使用中括号时，要在括号内使用属性名的字符串形式

```js
console.log(person["name"]); // "Nicholas"
console.log(person.name); // "Nicholas"
```

使用中括号的主要优势就是可以通过变量访问属性，另外，如果属性名中包含可能会导致语法错误的字符，或者包含关键字/保留字时，也可以使用中括号语法。

```js
let propertyName = "name";
console.log(person[propertyName]); // "Nicholas"

person["first name"] = "Nicholas";
```

> 通常，点语法是首选的属性存取方式，除非访问属性时必须使用变量

# Array

除了 Object，Array 应该就是 ECMAScript 中最常用的类型了。ECMAScript 数组跟其他编程语言的数组有很大区别。跟其他语言中的数组一样，ECMAScript 数组也是一组有序的数据，但跟其他语言不同的是，数组中每个槽位可以存储任意类型的数据

## 创建数组

有几种基本的方式可以创建数组

- 一种是使用 Array 构造函数

```js
let colors = new Array();
```

如果知道数组中元素的数量，那么可以给构造函数传入一个数值，然后 length 属性就会被自动创建并设置为这个值,也可以给 Array 构造函数传入要保存的元素

```js
let colors = new Array(20);

let colors = new Array("red", "blue", "green");
```

在使用 Array 构造函数时，也可以省略 new 操作符。结果是一样的

```js
let colors = Array(3); // 创建一个包含3 个元素的数组
let names = Array("Greg"); // 创建一个只包含一个元素，即字符串"Greg"的数组
```

- 另一种创建数组的方式是使用数组字面量（array literal）表示法

数组字面量是在中括号中包含以逗号分隔的元素列表

```js
let colors = ["red", "blue", "green"]; // 创建一个包含3 个元素的数组
let names = []; // 创建一个空数组
let values = [1, 2]; // 创建一个包含2 个元素的数组
```

> 与对象一样，在使用数组字面量表示法创建数组不会调用 Array 构造函数

**Array 构造函数还有两个 ES6 新增的用于创建数组的静态方法：from()和 of()**。from()用于将类数组结构转换为数组实例，而 of()用于将一组参数转换为数组实例

- Array.from()的第一个参数是一个类数组对象，即任何可迭代的结构，或者有一个 length 属性和可索引元素的结构

```js
// 字符串会被拆分为单字符数组
console.log(Array.from("Matt")); // ["M", "a", "t", "t"]
// 可以使用from()将集合和映射转换为一个新数组
const m = new Map().set(1, 2).set(3, 4);
const s = new Set().add(1).add(2).add(3).add(4);
console.log(Array.from(m)); // [[1, 2], [3, 4]]
console.log(Array.from(s)); // [1, 2, 3, 4]
// Array.from()对现有数组执行浅复制
const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1);
console.log(a1); // [1, 2, 3, 4]
alert(a1 === a2); // false
// 可以使用任何可迭代对象
const iter = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
  },
};
console.log(Array.from(iter)); // [1, 2, 3, 4]
// arguments 对象可以被轻松地转换为数组
function getArgsArray() {
  return Array.from(arguments);
}
console.log(getArgsArray(1, 2, 3, 4)); // [1, 2, 3, 4]
// from()也能转换带有必要属性的自定义对象
const arrayLikeObject = {
  0: 1,
  1: 2,
  2: 3,
  3: 4,
  length: 4,
};
console.log(Array.from(arrayLikeObject)); // [1, 2, 3, 4]
```

Array.from()还接收第二个可选的映射函数参数。这个函数可以直接增强新数组的值，而无须像调用 Array.from().map()那样先创建一个中间数组。还可以接收第三个可选参数，用于指定映射函数中 this 的值。但这个重写的 this 值在箭头函数中不适用

```js
const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1, (x) => x ** 2);
const a3 = Array.from(
  a1,
  function (x) {
    return x ** this.exponent;
  },
  { exponent: 2 }
);
console.log(a2); // [1, 4, 9, 16]
console.log(a3); // [1, 4, 9, 16]
```

- Array.of()可以把一组参数转换为数组。这个方法用于替代在 ES6 之前常用的 Array.prototype.slice.call(arguments)

```js
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
console.log(Array.of(undefined)); // [undefined]
```

## 数组空位

使用数组字面量初始化数组时，可以使用一串逗号来创建空位（hole）。ECMAScript 会将逗号之间相应索引位置的值当成空位，ES6 规范重新定义了该如何处理这些空位。

```js
const options = [, , , , ,]; // 创建包含5 个元素的数组
console.log(options.length); // 5
console.log(options); // [,,,,,]
```

ES6 新增的方法和迭代器与早期 ECMAScript 版本中存在的方法行为不同。ES6 新增方法普遍将这些空位当成存在的元素，只不过值为 undefined

ES6 之前的方法则会忽略这个空位，但具体的行为也会因方法而异

```js
const options = [1, , , , 5];
// map()会跳过空位置
console.log(options.map(() => 6)); // [6, undefined, undefined, undefined, 6]
// join()视空位置为空字符串
console.log(options.join("-")); // "1----5"
```

> 由于行为不一致和存在性能隐患，因此实践中要避免使用数组空位。如果确实需要空位，则可以显式地用 undefined 值代替

## 数组索引

要取得或设置数组的值，需要使用中括号并提供相应值的数字索引

```js
let colors = ["red", "blue", "green"]; // 定义一个字符串数组
alert(colors[0]); // 显示第一项
colors[2] = "black"; // 修改第三项
colors[3] = "brown"; // 添加第四项
```

在中括号中提供的索引表示要访问的值。如果索引小于数组包含的元素数，则返回存储在相应位置的元素，如果把一个值设置给超过数组最大索引的索引，则数组长度会自动扩展到该索引值加 1

数组中元素的数量保存在 length 属性中，这个属性始终返回 0 或大于 0 的值，数组 length 属性的独特之处在于，它不是只读的。**通过修改 length 属性，可以从数组末尾删除或添加元素**。

```js
let colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
colors.length = 2;
alert(colors[2]); // undefined
```

如果将 length 设置为大于数组元素数的值，则新添加的元素都将以 undefined 填充

```js
let colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
colors.length = 4;
alert(colors[3]); // undefined
```

使用 length 属性可以方便地向数组末尾添加元素

```js
let colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
colors[colors.length] = "black"; // 添加一种颜色（位置3）
colors[colors.length] = "brown"; // 再添加一种颜色（位置4）
```

数组中最后一个元素的索引始终是 length - 1

> 数组最多可以包含 4 294 967 295 个元素，这对于大多数编程任务应该足够了。如果
> 尝试添加更多项，则会导致抛出错误。以这个最大值作为初始值创建数组，可能导致脚本
> 运行时间过长的错误

## 检测数组

一个经典的 ECMAScript 问题是判断一个对象是不是数组

使用 instanceof 的问题是假定只有一个全局执行上下文。如果网页里有多个框架，则可能涉及两个不同的全局执行上下文，因此就会有两个不同版本的 Array 构造函数。如果要把数组从一个框架传给另一个框架，则这个数组的构造函数将有别于在第二个框架内本地创建的数组

为解决这个问题，ECMAScript 提供了 Array.isArray()方法。这个方法的目的就是确定一个值是否为数组，而不用管它是在哪个全局执行上下文中创建的

## 迭代器方法

在 ES6 中，Array 的原型上暴露了 3 个用于**检索数组内容的方法：keys()、values()和 entries()**。keys()返回数组索引的迭代器，values()返回数组元素的迭代器，而 entries()返回索引/值对的迭代器

```js
const a = ["foo", "bar", "baz", "qux"];
// 因为这些方法都返回迭代器，所以可以将它们的内容
// 通过Array.from()直接转换为数组实例
const aKeys = Array.from(a.keys());
const aValues = Array.from(a.values());
const aEntries = Array.from(a.entries());
console.log(aKeys); // [0, 1, 2, 3]
console.log(aValues); // ["foo", "bar", "baz", "qux"]
console.log(aEntries); // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]]
```

使用 ES6 的解构可以非常容易地在循环中拆分键/值对

```js
const a = ["foo", "bar", "baz", "qux"];
for (const [idx, element] of a.entries()) {
  alert(idx);
  alert(element);
}
// 0
// foo
// 1
// bar
// 2
// baz
// 3
// qux
```

## 复制和填充方法

ES6 新增了两个方法：**批量复制方法 copyWithin()，以及填充数组方法 fill()**。这两个方法的函数签名类似，都需要指定既有数组实例上的一个范围，包含开始索引，不包含结束索引。使用这个方法不会改变数组的大小。
使用**fill()方法可以向一个已有的数组中插入全部或部分相同的值**。开始索引用于指定开始填充的位置，它是可选的。如果不提供结束索引，则一直填充到数组末尾。负值索引从数组末尾开始计算。也可以将负索引想象成数组长度加上它得到的一个正索引

```js
const zeroes = [0, 0, 0, 0, 0];
// 用5 填充整个数组
zeroes.fill(5);
console.log(zeroes); // [5, 5, 5, 5, 5]
zeroes.fill(0); // 重置
// 用6 填充索引大于等于3 的元素
zeroes.fill(6, 3);
console.log(zeroes); // [0, 0, 0, 6, 6]
zeroes.fill(0); // 重置
// 用7 填充索引大于等于1 且小于3 的元素
zeroes.fill(7, 1, 3);
console.log(zeroes); // [0, 7, 7, 0, 0];
zeroes.fill(0); // 重置
// 用8 填充索引大于等于1 且小于4 的元素
// (-4 + zeroes.length = 1)
// (-1 + zeroes.length = 4)
zeroes.fill(8, -4, -1);
console.log(zeroes); // [0, 8, 8, 8, 0];
fill()静默忽略超出数组边界、零长度及方向相反的索引范围：
const zeroes = [0, 0, 0, 0, 0];
// 索引过低，忽略
zeroes.fill(1, -10, -6);
console.log(zeroes); // [0, 0, 0, 0, 0]
// 索引过高，忽略
zeroes.fill(1, 10, 15);
console.log(zeroes); // [0, 0, 0, 0, 0]
// 索引反向，忽略
zeroes.fill(2, 4, 2);
console.log(zeroes); // [0, 0, 0, 0, 0]
// 索引部分可用，填充可用部分
zeroes.fill(4, 3, 10)
console.log(zeroes); // [0, 0, 0, 4, 4]
```

与 fill()不同，**copyWithin()会按照指定范围浅复制数组中的部分内容，然后将它们插入到指定索引开始的位置**。开始索引和结束索引则与 fill()使用同样的计算方法

```js
let ints,
  reset = () => (ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);
reset();
// 从ints 中复制索引0 开始的内容，插入到索引5 开始的位置
// 在源索引或目标索引到达数组边界时停止
ints.copyWithin(5);
console.log(ints); // [0, 1, 2, 3, 4, 0, 1, 2, 3, 4]
reset();
// 从ints 中复制索引5 开始的内容，插入到索引0 开始的位置
ints.copyWithin(0, 5);
console.log(ints); // [5, 6, 7, 8, 9, 5, 6, 7, 8, 9]
reset();
// 从ints 中复制索引0 开始到索引3 结束的内容
// 插入到索引4 开始的位置
ints.copyWithin(4, 0, 3);
alert(ints); // [0, 1, 2, 3, 0, 1, 2, 7, 8, 9]
reset();
// JavaScript 引擎在插值前会完整复制范围内的值
// 因此复制期间不存在重写的风险
ints.copyWithin(2, 0, 6);
alert(ints); // [0, 1, 0, 1, 2, 3, 4, 5, 8, 9]
reset();
// 支持负索引值，与fill()相对于数组末尾计算正向索引的过程是一样的
ints.copyWithin(-4, -7, -3);
alert(ints); // [0, 1, 2, 3, 4, 5, 3, 4, 5, 6]
```

copyWithin()静默忽略超出数组边界、零长度及方向相反的索引范围：

```js
let ints,
  reset = () => (ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);
reset();
// 索引过低，忽略
ints.copyWithin(1, -15, -12);
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
reset();
// 索引过高，忽略
ints.copyWithin(1, 12, 15);
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
reset();
// 索引反向，忽略
ints.copyWithin(2, 4, 2);
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
reset();
// 索引部分可用，复制、填充可用部分
ints.copyWithin(4, 7, 10);
alert(ints); // [0, 1, 2, 3, 7, 8, 9, 7, 8, 9];
```

## 转换方法

前面提到过，所有对象都有 toLocaleString()、toString()和 valueOf()方法。其中，valueOf()返回的还是数组本身。而 toString()返回由数组中每个值的等效字符串拼接而成的一个逗号分隔的字符串。也就是说，对数组的每个值都会调用其 toString()方法，以得到最终的字符串

```js
let colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
alert(colors.toString()); // red,blue,green
alert(colors.valueOf()); // red,blue,green
alert(colors); // red,blue,green
```

因为 alert()期待字符串，所以会在后台调用数组的 toString()方法，从而得到跟前面一样的结果

toLocaleString()方法也可能返回跟 toString()和 valueOf()相同的结果，但也不一定。在调用数组的 toLocaleString()方法时，会得到一个逗号分隔的数组值的字符串。它与另外两个方法唯一的区别是，为了得到最终的字符串，会调用数组每个值的 toLocaleString()方法，而不是 toString()方法

```js
let person1 = {
  toLocaleString() {
    return "Nikolaos";
  },
  toString() {
    return "Nicholas";
  },
};
let person2 = {
  toLocaleString() {
    return "Grigorios";
  },
  toString() {
    return "Greg";
  },
};
let people = [person1, person2];
alert(people); // Nicholas,Greg
alert(people.toString()); // Nicholas,Greg
alert(people.toLocaleString()); // Nikolaos,Grigorios
```

继承的方法 toLocaleString()以及 toString()都返回数组值的逗号分隔的字符串。如果想使用不同的分隔符，则可以使用 join()方法。

join()方法接收一个参数，即字符串分隔符，返回包含所有项的字符串

```js
let colors = ["red", "green", "blue"];
alert(colors.join(",")); // red,green,blue
alert(colors.join("||")); // red||green||blue
```

这里在 colors 数组上调用了 join()方法，得到了与调用 toString()方法相同的结果。如果不给 join()传入任何参数，或者传入 undefined，则仍然使用逗号作为分隔符。

> 如果数组中某一项是 null 或 undefined，则在 join()、toLocaleString()、toString()和 valueOf()返回的结果中会以空字符串表示

## 栈方法

数组对象可以像栈一样，
也就是一种限制插入和删除项的数据结构。栈是一种后进先出（LIFO，Last-In-First-Out）的结构，也就是最近添加的项先被删除。数据项的插入（称为推入，push）和删除（称为弹出，pop）只在栈的一个地方发生，即栈顶。ECMAScript 数组提供了 push()和 pop()方法，以实现类似栈的行为。

push()方法接收任意数量的参数，并将它们添加到数组末尾，返回数组的最新长度。pop()方法则用于删除数组的最后一项，同时减少数组的 length 值，返回被删除的项

```js
let colors = new Array(); // 创建一个数组
let count = colors.push("red", "green"); // 推入两项
alert(count); // 2
count = colors.push("black"); // 再推入一项
alert(count); // 3
let item = colors.pop(); // 取得最后一项
alert(item); // black
alert(colors.length); // 2
```

这里创建了一个当作栈来使用的数组（注意不需要任何额外的代码，push()和 pop()都是数组的默认方法

在调用 pop()时，会返回数组的最后一项

栈方法可以与数组的其他任何方法一起使用

```js
let colors = ["red", "blue"];
colors.push("brown"); // 再添加一项
colors[3] = "black"; // 添加一项
alert(colors.length); // 4
let item = colors.pop(); // 取得最后一项
alert(item); // black
```

## 队列方法

要模拟队列就差一个从数组开头取得数据的方法了。这个数组方法叫 shift()，它会删除数组的第一项并返回它，然后数组长度减 1。使用 shift()和 push()，可以把数组当成队列来使用

```js
let colors = new Array(); // 创建一个数组
let count = colors.push("red", "green"); // 推入两项
alert(count); // 2
count = colors.push("black"); // 再推入一项
alert(count); // 3
let item = colors.shift(); // 取得第一项
alert(item); // red
alert(colors.length); // 2
```

ECMAScript 也为数组提供了 unshift()方法。顾名思义，unshift()就是执行跟 shift()相反的操作：在数组开头添加任意多个值，然后返回新的数组长度。通过使用 unshift()和 pop()，可以在相反方向上模拟队列，即在数组开头添加新数据，在数组末尾取得数据

```js
let colors = new Array(); // 创建一个数组
let count = colors.unshift("red", "green"); // 从数组开头推入两项
alert(count); // 2
count = colors.unshift("black"); // 再推入一项
alert(count); // 3
let item = colors.pop(); // 取得最后一项
alert(item); // green
alert(colors.length); // 2
```

## 排序方法

数组有两个方法可以用来对元素重新排序：reverse()和 sort()。顾名思义，reverse()方法就是将数组元素反向排列

```js
let values = [1, 2, 3, 4, 5];
values.reverse();
alert(values); // 5,4,3,2,1
```

数组 values 的初始状态为[1,2,3,4,5]。通过调用 reverse()反向排序，得到了[5,4,3,2,1]。这个方法很直观，但不够灵活，所以才有了 sort()方法

默认情况下，sort()会按照升序重新排列数组元素，即最小的值在前面，最大的值在后面。为此，sort()会在每一项上调用 String()转型函数，然后比较字符串来决定顺序

```js
let values = [0, 1, 5, 10, 15];
values.sort();
alert(values); // 0,1,10,15,5
```

sort()方法可以接收一个比较函数，用于判断哪个值应该排在前面

比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，就返回负值；如果两个参数相等，就返回 0；如果第一个参数应该排在第二个参数后面，就返回正值。

```js
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}
```

这个比较函数可以适用于大多数数据类型，可以把它当作参数传给 sort()方法，如下所示：

```js
let values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); // 0,1,5,10,15
```

在给 sort()方法传入比较函数后，数组中的数值在排序后保持了正确的顺序。当然，比较函数也可以产生降序效果，只要把返回值交换一下即可

```js
function compare(value1, value2) {
  if (value1 < value2) {
    return 1;
  } else if (value1 > value2) {
    return -1;
  } else {
    return 0;
  }
}
let values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); // 15,10,5,1,0
```

此外，这个比较函数还可简写为一个箭头函数：

```js
let values = [0, 1, 5, 10, 15];
values.sort((a, b) => (a < b ? 1 : a > b ? -1 : 0));
alert(values); // 15,10,5,1,0
```

> reverse()和 sort()都返回调用它们的数组的引用

## 操作方法

- concat()方法可以在现有数组全部元素基础上创建一个新数组

```js
let colors = ["red", "green", "blue"];
let colors2 = colors.concat("yellow", ["black", "brown"]);
console.log(colors); // ["red", "green","blue"]
console.log(colors2); // ["red", "green", "blue", "yellow", "black", "brown"]
```

打平数组参数的行为可以重写，方法是在参数数组上指定一个特殊的符号：Symbol.isConcat-Spreadable。这个符号能够阻止 concat()打平参数数组。相反，把这个值设置为 true 可以强制打平类数组对象

```js
let colors = ["red", "green", "blue"];
let newColors = ["black", "brown"];
let moreNewColors = {
  [Symbol.isConcatSpreadable]: true,
  length: 2,
  0: "pink",
  1: "cyan",
};
newColors[Symbol.isConcatSpreadable] = false;
// 强制不打平数组
let colors2 = colors.concat("yellow", newColors);
// 强制打平类数组对象
let colors3 = colors.concat(moreNewColors);
console.log(colors); // ["red", "green", "blue"]
console.log(colors2); // ["red", "green", "blue", "yellow", ["black", "brown"]]
console.log(colors3); // ["red", "green", "blue", "pink", "cyan"]
```

- ，方法 slice()用于创建一个包含原有数组中一个或多个元素的新数组

slice()方法可以接收一个或两个参数：返回元素的开始索引和结束索引。如果只有一个参数，则 slice()会返回该索引到数组末尾的所有元素。如果有两个参数，则 slice()返回从开始索引到结束索引对应的所有元素，其中不包含结束索引对应的元素。记住，这个操作不影响原始数组。

```js
let colors = ["red", "green", "blue", "yellow", "purple"];
let colors2 = colors.slice(1);
let colors3 = colors.slice(1, 4);
alert(colors2); // green,blue,yellow,purple
alert(colors3); // green,blue,yellow
```

> 如果 slice()的参数有负值，那么就以数值长度加上这个负值的结果确定位置。比如，在包含 5 个元素的数组上调用 slice(-2,-1)，就相当于调用 slice(3,4)。如果结束位置小于开始位置，则返回空数组。

- 或许最强大的数组方法就属 splice(),splice()的主要目的是在数组中间插入元素

1. 删除。需要给 splice()传 2 个参数：要删除的第一个元素的位置和要删除的元素数量。可以从数组中删除任意多个元素，比如 splice(0, 2)会删除前两个元素。
2. 插入。需要给 splice()传 3 个参数：开始位置、0（要删除的元素数量）和要插入的元素，可以在数组中指定的位置插入元素。第三个参数之后还可以传第四个、第五个参数，乃至任意多个要插入的元素。比如，splice(2, 0, "red", "green")会从数组位置 2 开始插入字符串
   "red"和"green"。
3. 替换。splice()在删除元素的同时可以在指定位置插入新元素，同样要传入 3 个参数：开始位置、要删除元素的数量和要插入的任意多个元素。要插入的元素数量不一定跟删除的元素数量一致。比如，splice(2, 1, "red", "green")会在位置 2 删除一个元素，然后从该位置开始向数组中插入"red"和"green"。

splice()方法始终返回这样一个数组，它包含从数组中被删除的元素（如果没有删除元素，则返回空数组）

```js
let colors = ["red", "green", "blue"];
let removed = colors.splice(0, 1); // 删除第一项
alert(colors); // green,blue
alert(removed); // red，只有一个元素的数组
removed = colors.splice(1, 0, "yellow", "orange"); // 在位置1 插入两个元素
alert(colors); // green,yellow,orange,blue
alert(removed); // 空数组
removed = colors.splice(1, 1, "red", "purple"); // 插入两个值，删除一个元素
alert(colors); // green,red,purple,orange,blue
alert(removed); // yellow，只有一个元素的数组
```

## 搜索和位置方法

ECMAScript 提供两类搜索数组的方法：按严格相等搜索和按断言函数搜索。

- 严格相等

ECMAScript 提供了**3 个严格相等的搜索方法：indexOf()、lastIndexOf()和 includes()**。其中，前两个方法在所有版本中都可用，而第三个方法是 ECMAScript 7 新增的。这些方法都接收两个参数：要查找的元素和一个可选的起始搜索位置。indexOf()和 includes()方法从数组前头（第一项）开始向后搜索，而 lastIndexOf()从数组末尾（最后一项）开始向前搜索。

indexOf()和 lastIndexOf()都返回要查找的元素在数组中的位置，如果没找到则返回 -1。includes()返回布尔值，表示是否至少找到一个与指定元素匹配的项。在比较第一个参数跟数组每一项时，会使用全等（===）比较，也就是说两项必须严格相等。

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
alert(numbers.indexOf(4)); // 3
alert(numbers.lastIndexOf(4)); // 5
alert(numbers.includes(4)); // true
alert(numbers.indexOf(4, 4)); // 5
alert(numbers.lastIndexOf(4, 4)); // 3
alert(numbers.includes(4, 7)); // false
let person = { name: "Nicholas" };
let people = [{ name: "Nicholas" }];
let morePeople = [person];
alert(people.indexOf(person)); // -1
alert(morePeople.indexOf(person)); // 0
alert(people.includes(person)); // false
alert(morePeople.includes(person)); // true
```

- 断言函数

ECMAScript 也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回值决定了相应索引的元素是否被认为匹配。

断言函数接收 3 个参数：元素、索引和数组本身。其中元素是数组中当前搜索的元素，索引是当前元素的索引，而数组就是正在搜索的数组。断言函数返回真值，表示是否匹配。

find()和 findIndex()方法使用了断言函数。这两个方法都从数组的最小索引开始。find()返回第一个匹配的元素，findIndex()返回第一个匹配元素的索引。这两个方法也都接收第二个可选的参数，用于指定断言函数内部 this 的值

```js
const people = [
  {
    name: "Matt",
    age: 27,
  },
  {
    name: "Nicholas",
    age: 29,
  },
];
alert(people.find((element, index, array) => element.age < 28));
// {name: "Matt", age: 27}
alert(people.findIndex((element, index, array) => element.age < 28));
// 0
```

找到匹配项后，这两个方法都不再继续搜索

```js
const evens = [2, 4, 6];
// 找到匹配后，永远不会检查数组的最后一个元素
evens.find((element, index, array) => {
  console.log(element);
  console.log(index);
  console.log(array);
  return element === 4;
});
// 2
// 0
// [2, 4, 6]
// 4
// 1
// [2, 4, 6]
```

## 迭代方法

ECMAScript 为数组定义了 5 个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数，以及可选的作为函数运行上下文的作用域对象（影响函数中 this 的值）。传给每个方法的函数接收 3 个参数：数组元素、元素索引和数组本身

- every()：对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。
- filter()：对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回。
- forEach()：对数组每一项都运行传入的函数，没有返回值。
- map()：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。
- some()：对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。

every()和 some()是最相似的，都是从数组中搜索符合某个条件的元素。对 every()
来说，传入的函数必须对每一项都返回 true，它才会返回 true；否则，它就返回 false。而对 some()来说，只要有一项让传入的函数返回 true，它就会返回 true

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let everyResult = numbers.every((item, index, array) => item > 2);
alert(everyResult); // false
let someResult = numbers.some((item, index, array) => item > 2);
alert(someResult); // true
```

看一看 filter()方法。这个方法基于给定的函数来决定某一项是否应该包含在它返回的数
组中。

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let filterResult = numbers.filter((item, index, array) => item > 2);
alert(filterResult); // 3,4,5,4,3
```

这个方法非常适合从数组中筛选满足给定条件的元素

map()方法也会返回一个数组。这个数组的每一项都是对原始数组中同样位置的元素运行传入函数而返回的结果。

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let mapResult = numbers.map((item, index, array) => item * 2);
alert(mapResult); // 2,4,6,8,10,8,6,4,2
```

这个方法非常适合创建一个与原始数组元素一一对应的新数组

看一看 forEach()方法。这个方法只会对每一项运行传入的函数，没有返回值。本质上，forEach()方法相当于使用 for 循环遍历数组

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.forEach((item, index, array) => {
  // 执行某些操作
});
```

数组的这些迭代方法通过执行不同操作方便了对数组的处理

## 归并方法

ECMAScript 为**数组提供了两个归并方法：reduce()和 reduceRight()**。这两个方法都会迭代数组的所有项，并在此基础上构建一个最终返回值。reduce()方法从数组第一项开始遍历到最后一项。而 reduceRight()从最后一项开始遍历至第一项。

这两个方法都接收两个参数：对每一项都会运行的归并函数，以及可选的以之为归并起点的初始值。传给 reduce()和 reduceRight()的函数接收 4 个参数：上一个归并值、当前项、当前项的索引和数组本身。

可以使用 reduce()函数执行累加数组中所有数值的操作

```js
let values = [1, 2, 3, 4, 5];
let sum = values.reduce((prev, cur, index, array) => prev + cur);
alert(sum); // 15
```

如此递进，直到把所有项都遍历一次，最后返回归并结果。reduceRight()方法与之类似，只是方向相反。

```js
let values = [1, 2, 3, 4, 5];
let sum = values.reduceRight(function (prev, cur, index, array) {
  return prev + cur;
});
alert(sum); // 15
```
    
当然，最终结果相同，因为归并操作都是简单的加法。

究竟是使用 reduce()还是 reduceRight()，只取决于遍历数组元素的方向。除此之外，这两个方法没什么区别
