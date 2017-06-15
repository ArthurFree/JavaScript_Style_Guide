# JavaScript Style Guide

## 目录

1. 类型(Types)
2. 引用(References)
3. 对象(Objects)
4. 数组(Arrays)
5. (Destructuring)
6. 字符串(String)
7. 函数(Functions)
8. 箭头函数(Arrow Functions)
9. 类 & Constructors方法(classes & Constructors)
10. 模块(Modules)
11. 遍历器 & Generators(Iterators and Generators)
12. 属性(Properties)
13. 变量(Variables)
14. 提升(Hoisting)
15. 比较运算符 & 等号(Comparison Operators & Equality)
16. 块(Blocks)
17. 控制声明(Control Statements)
18. 注释(Comments)
19. 空白(Whitespace)
20. 逗号(Commas)
21. 分号(Semicolons)
22. 类型转化 & 强制 (Type Casting & Coercion)
23. 命名规则(Naming Conventions)
24. 存取器(Accessors)
25. 事件(Events)
26. jQuery(jQuery)
27. ES5兼容性(ECMAScript 5 Compatibility)
28. (ECMAScript 6 + (ES 205+) Styles)
29. (Testing)
30. (Performance)
31. (Resources)
32. (In the Wild)
33. (Translation)
34. (The JavaScript Style Guide)

## 类型

-   基本类型值： 当你存取一个基本类型时，你操作的直接是它的值
    -   `string`
    -   `number`
    -   `boolean`
    -   `null`
    -   `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

-   复杂类型值：当你存取一个复杂类型时，你操作的是它的值得引用
    -   `object`
    -   `array`
    -   `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 0
    ```

## 引用

-   为你的所有的引用使用`const`；避免使用`var`,eslint规则：`perfer-const`,`no-const-assign`
    > 为什么？确保你不会重新赋值你的引用，导致出现bug和难以理解的代码

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

-   如果必须重新赋值引用，使用`let`来代替`var`,eslint规则：`no-var`, jscs:`disallowVar`
    > 为什么？`let`是块作用域而不是像`var`一样是函数作用域

    ```javascript
    // bad
    var count = 1;
    if (true) {
        count+= 1;
    }

    // good
    let count = 1;
    if (true) {
        count += 1;
    }
    ```

-   注意`let`和`const`都是块级作用域

    ```javascript
    // const 和 let 只存在于定义他们的块内部
    {
        let a = 1;
        const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

## 对象

-   使用字面量语法来创建对象, eslint规则：`no-new-object`

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

-   当创建有动态属性的名称的对象时，使用计算属性
    > 为什么？这样允许你在一个地方定义此对象所有的属性

    ```javascript
    function getKey(k) {
        return `a Key named ${k}`;
    }

    // bad
    const obj = {
        id: 5,
        name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
        id: 5,
        name: 'San Francisco',
        [getKey('enabled')]: true,
    };
    ```

-   使用对象方法简写,eslint规则：`object-shorthand`,jscs:`requireEnhancedObjectLiterals`

    ```javascript
    // bad
    const atom = {
        value: 1,
        addValue: function (value) {
            return atom.value + value;
        },
    };

    // good
    const atom = {
        value: 1,
        addValue(value) {
            return atom.value + value;
        },
    };
    ```

-   使用属性值简写,eslint规则：`object-shorthand`,jscs:`requireEnhancedObjectLiterals`
    > 为什么？书写和描述更短

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
    };
    ```

-   将所有使用简写方法的属性名放在一起，并放在声明的对象的最上面
    > 为什么？这样更容易说明哪些属性使用了简写

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        episodeOne: 1,
        twoJediWalkIntoACantina: 2,
        lukeSkywalker,
        episodeThree: 3,
        mayTheFourth: 4,
        anakinSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
        anakinSkywalker,
        episodeOne: 1,
        twoJediWalkIntoACantina: 2,
        episodeThree: 3,
        mayTheFourth: 4,
    };
    ```

-   只对无效标识符使用单引号,eslint规则：`quote-props`,jscs:`disallowQuoteKeysInObjects`
    > 为什么？通常，我们认为它主观上更易读.它增强了语法高亮，并且更容易被js引擎优化

    ```javascript
    // bad
    const bad = {
        'foo': 3,
        'bar': 4,
        'data-blah: 5,
    };

    // good
    const good = {
        foo: 3,
        bar: 4,
        'data-blah: 5,
    };
    ```

-   不要直接调用`Object.prototype`的方法，例如`hasOwnProperty`,`propertyIsEnumerable`,and `isPrototypeOf`.
    > 为什么？这些方法可能被当前对象上的属性所覆盖 - 考虑 `{ hasOwnProperty: false }` - 或者，当此对象是 `null`时(`Object.create(null)`)

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // 在模块作用域内，缓存查找一次
    /* or */
    import has from 'has';
    // ...
    console.log(has.call(object, key));
    ```

-   使用对象扩展操作符(`{...}`)来代替`Object.assign`实现浅拷贝。使用对象的取余操作符来得到一个新的对象使某些属性省略

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 });
    delete copy.a;

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

## 数组

-   使用字面量语法来创建数组.eslint规则：`no-array-constructor`

    ```javascript
    // bad
    const item = new Array()

    // good
    const item = [];
    ```

-   使用数组的`push`方法代替直接在数组中为某一项通过赋值方法进行添加操作

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

-   使用数组扩展操作符(`...`)复制数组

    ```javascript
    // bad
    const len = item.length;
    const itemCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
        itemCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...item];
    ```

-   转换类数组对象成数组，使用`Array.from`

    ```javascript
    const foo = document.querySleectorAll('.foo');
    const nodes = Array.from(foo);
    ```

-   在数组方法的回调函数中使用`return`语句.如果函数体返回一个由一个简单语句组成没有副作用的表达式，可以省略`return`, 参考8.2, eslint规则：`array-callback-return`

    ```javascript
    // good
    [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
        const flatten = memo.concat(item);
        flat[index] = flatten;
    });

    // good
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
        const flatten = memo.concat(item);
        flat[index] = flatten;
        return flatten
    });

    // bad
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === 'Mockingbird') {
            return author === 'Harper Lee';
        } else {
            return false;
        }
    });

    // good
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === 'Mockingbird') {
            return author === 'Harper Lee';
        }

        return false;
    });
    ```

-   如果数组有多行，在数组开括号后面，闭括号前面换行

    ```javascript
    // bad
    const arr = [
        [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
        id: 1,
    }, {
        id: 2,
    }];

    const numberInArray = [
        1, 2,
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
        {
            id: 1,
        },
        {
            id: 2,
        },
    ];

    const numberInArray = [
        1,
        2,
    ];
    ```

## 解构

-   当在一个对象中存取和使用多个属性时，使用对象解构.jscs:`requireObjectDestructuring`
    > 为什么？解构可以保存你为这些属性创建的临时引用

    ```javascript
    // bad
    function getFullName(user) {
        const firstName = user.firstName;
        const lastName = user.lastName;

        return `${firstName}${lastName}`;
    }

    // good
    function getFullName(user) {
        const { firstName, lastName } = user;

        return `${firstName}${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
        return `${firstName}${lastName}`;
    }
    ```

-   使用数组结构.jscs:`requireArrayDestructuring`

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    coinst second = arr[1];

    // good
    const [first, second] = arr;
    ```

-   对多个返回值使用对象解构，而不用数组结构.jscs:`disallowArrayDestructuringReturn`
    > 为什么？在以后你可以添加新的属性或者改变顺序而不会打断其调用的地方

    ```javascript
    // bad
    function processInput(input) {
        return [left, right, top, bottom];
    }

    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
        return { left, right, top, bottom };
    }

    const { left, top } = processInput(input);
    ```

## 字符串

-   字符串使用单引号(`''`).eslint规则：`quotes`,jscs:`validateQuoteMarks`

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - 模板字符串应该包含插值或者换行
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

-   字符串一行超过100个字符时不应该使用字符串连接写成多行
    > 为什么？散碎的字符串书写麻烦并且降低了代码的可搜索性

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
        'of Batman. When you stop to think about how Batman had anything to do ' +
        'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

-   当以编程的方式构建字符串时，使用模板字符串代替字符串拼接.eslint规则：`perfer-template`、`template-curly-spacing`,jscs:`requireTemplateStrings`
    > 为什么？模板字符串更易读，恰当的换行与字符串插值使语法更简洁

    ```javascript
    // bad
    function sayHi(name) {
        return 'How are you,' + name + '?';
    }

    // bad
    function sayHi(name) {
        return ['How are you,', name, '?'].join();
    }

    // bad
    function sayHi(name) {
        retrun `How are you, ${ name }`;
    }

    // good
    function sayHi(name) {
        return `How are you, ${name}`;
    }
    ```

-   永远不要在字符串上使用`eval()`，它会导致很多的漏洞.eslint规则：`no-eval`

-   在字符串中不要出现没有必要的转义字符串.eslint规则：`no-useless-escape`
    > 为什么？反斜杠降低了代码的可读性，所以应该只在必要的时候使用

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"'

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```
