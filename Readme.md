# Node.js代码风格指南

This is a guide for writing consistent and aesthetically pleasing node.js code.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

项目中有个.jshintrc文件用来在jshint中检查代码是否符合这些规则。
There is a .jshintrc which enforces these rules as closely as possible. You can
either use that and adjust it, or use
[this script](https://gist.github.com/kentcdodds/11293570) to make your own.

This guide was created by [Felix Geisendörfer](http://felixge.de/) and is
licensed under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## 目录

### 格式 Formatting
* [缩进采用2个空格](#缩进采用2个空格)
* [换行](#换行)
* [行末不能有空格](#行末不能有空格)
* [语句结尾不要故意漏掉分号](#语句结尾不要故意漏掉分号)
* [每行最多80个字符](#每行最多80个字符)
* [使用单引号](#使用单引号)
* [开头大括号在同一行内](#开头大括号在同一行内)
* [一个var只对应声明一个变量](#一个var只对应声明一个变量)

### 命名规范 Naming Conventions
* [变量、属性和函数名使用小写开头的驼峰命名法](#use-lowercamelcase-for-variables-properties-and-function-names)
* [类名使用大写开头的驼峰命名法](#use-uppercamelcase-for-class-names)
* [常量使用大写](#use-uppercase-for-constants)

### 变量 Variables
* [创建对象和数组的写法](#object--array-creation)

### 条件判断 Conditionals
* [使用 === 操作符](#use-the--operator)
* [使用多行的三元操作符](#use-multi-line-ternary-operator)
* [使用描述命名的条件表达式](#use-descriptive-conditions)

### 函数 Functions
* [函数应该保持简短](#write-small-functions)
* [在函数中尽早返回](#return-early-from-functions)
* [对闭包命名](#name-your-closures)
* [不要有嵌套的闭包](#no-nested-closures)
* [方法链式调用](#method-chaining)

### 注释 Comments
* [使用//表示备注](#use-slashes-for-comments)

### 其他 Miscellaneous
* [Object.freeze, Object.preventExtensions, Object.seal, with, eval](#objectfreeze-objectpreventextensions-objectseal-with-eval)
* [Require放在顶部](#requires-at-top)
* [Getter和setter](#getters-and-setters)
* [不要扩展原生的prototypes](#do-not-extend-built-in-prototypes)

## 格式 Formatting

你可以用 [editorconfig.org](http://editorconfig.org/) 来设置你的编辑器的代码风格. 使用 [Node.js Style Guide .editorconfig file](.editorconfig)来自动检查风格符合下面的缩进、换行、空格规范。

### 缩进采用2个空格

使用2个空格来进行缩进，对天发誓你不会把空格按成tab。否则你代码会出现很奇葩的bug。

### 换行

Use UNIX-style newlines (`\n`), and a newline character as the last character
of a file. Windows-style newlines (`\r\n`) are forbidden inside any repository.

### 行末不能有空格

行末不能带有空格，否则你代码很难看而且会出错。webstorm之类的IDE和jshint/eslint之类的代码静态检查都会提示。

### 语句结尾不要故意漏掉分号

根据 [hackernews上的问卷调查][hnsemicolons], 使用分号是JS社区的原则。 可以了解考虑 [反面观点][the opposition]，但在追求简单的语法快感和过度依赖错误检查机制这两者的权衡上还是应该保守一些。

[the opposition]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hnsemicolons]: http://news.ycombinator.com/item?id=1547647

### 每行最多80个字符

Limit your lines to 80 characters. Yes, screens have gotten much bigger over the
last few years, but your brain has not. Use the additional room for split screen,
your editor supports that, right?

### 使用单引号

Use single quotes, unless you are writing JSON.

*Right:*

```js
var foo = 'bar';
```

*Wrong:*

```js
var foo = "bar";
```

### 开头大括号在同一行内

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

Also, notice the use of whitespace before and after the condition statement.

### 一个var只对应声明一个变量

Declare one variable per var statement, it makes it easier to re-order the
lines. However, ignore [Crockford][crockfordconvention] when it comes to
declaring variables deeper inside a function, just put the declarations wherever
they make sense.

*Right:*

```js
var keys   = ['foo', 'bar'];
var values = [23, 42];

var object = {};
while (keys.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (keys.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

[crockfordconvention]: http://javascript.crockford.com/code.html

### Naming Conventions

### Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use `lowerCamelCase`.  They
should also be descriptive. Single character variables and uncommon
abbreviations should generally be avoided.

*Right:*

```js
var adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
var admin_user = db.query('SELECT * FROM users ...');
```

### Use UpperCamelCase for class names

Class names should be capitalized using `UpperCamelCase`.

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties,
using all uppercase letters.

*Right:*

```js
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Variables

### Object / Array creation

object的每一行key-value后面都带上逗号。如果对象很简单，书写时就不要换行。key不要带引号，除非key中间有空格:

*正确:*

```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```

*错误:*

```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## Conditionals

### Use the === operator

[相等性的判断规则][comparisonoperators]容易造成歧义. 使用===来判断对象相等，这样才靠谱.

*正确:*

```js
var a = 0;
if (a !== '') {
  console.log('winning');
}

```

*错误:*

```js
var a = 0;
if (a == '') {
  console.log('losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

### Use multi-line ternary operator

三元表达式不应该写在同一行。条件复杂的情况下应该写成多行。

*推荐:*

```js
var foo = (a === b)
  ? 1
  : 2;
```

*简单情况下:*

```js
var foo = (a === b) ? 1 : 2;
```

### Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

*Right:*

```js
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
  console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  console.log('losing');
}
```

## Functions

### Write small functions

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

### Return early from functions

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

*Right:*

```js
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

### Name your closures

匿名回调函数（闭包）可以改成有名字的函数。这样在调试查看函数调用堆栈、profile内存堆栈和CPU的时候都会更直观。

*正确:*

```js
req.on('end', function onEnd() {
  console.log('winning');
});
```

*错误:*

```js
req.on('end', function() {
  console.log('losing');
});
```

### No nested closures

写代码的时候不要把闭包（匿名回调函数）嵌套在一起，否则你的代码会很难看。

*正确:*

```js
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

*错误:*

```js
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```


### Method chaining

链式调用方法的时候一行只写一个方法，并且.开头要缩进表示这是链式调用当中。

*Right:*

```js
User
  .findOne({ name: 'foo' })
  .populate('bar')
  .exec(function(err, user) {
    return true;
  });
```

*Wrong:*

```js
User
.findOne({ name: 'foo' })
.populate('bar')
.exec(function(err, user) {
  return true;
});

User.findOne({ name: 'foo' })
  .populate('bar')
  .exec(function(err, user) {
    return true;
  });

User.findOne({ name: 'foo' }).populate('bar')
.exec(function(err, user) {
  return true;
});

User.findOne({ name: 'foo' }).populate('bar')
  .exec(function(err, user) {
    return true;
  });
```

## Comments

### Use slashes for comments

单行和多行的注释里都用//来表示（IDE有注释快捷键）。注释应该用来说明不那么浅显的设计或者原理，不要过度注释那些直接能看懂的代码。

*Right:*

```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

*Wrong:*

```js
// 执行正则表达式
var matches = item.match(/ID_([^\n]+)=([^\n]+)/);

// 使用方法: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```

## Miscellaneous

### Object.freeze, Object.preventExtensions, Object.seal, with, eval

不要用上面这些疯狂的东西，基本上你都用不到，只会让你的代码更看不懂。

### Requires At Top

require要放在文件开头，这样才能清晰的表明文件的依赖关系。Besides giving an overview for others at a quick glance of dependencies and possible memory impact, it allows one to determine if they need a package.json file should they choose to use the file elsewhere.

### Getters and setters

尽量不要使用setter（在赋值的时候自定义修改内部属性），这会给用你代码的人带来更多问题。（object应该尽量是immutable，避免修改内部属性这种[副作用side effect][sideeffect]）

没有[副作用side effects][sideeffect]的getter可以用, 比如给一个自定义的集合类Collection设置一个length属性来计算集合大小。

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)

### Do not extend built-in prototypes

不要为了装逼扩展原生的js对象的prototype。未来的你自己会感激你的。

*正确:*

```js
var a = [];
if (!a.length) {
  console.log('winning');
}
```

*错误:*

```js
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}
```


