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
    ```
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
-   复杂类型值：当你存取一个复杂类型时，你操作的是它的值得引用
    -   `object`
    -   `array`
    -   `function`
    ```
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 0
    ```

## 引用

-   为你的所有的引用使用`const`；避免使用`var`,eslint规则：`perfer-const`,`no-const-assign`
    > 为什么？确保你不会重新赋值你的引用，导致出现bug和难以理解的代码
    ```
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```
-   如果必须重新赋值引用，使用`let`来代替`var`,eslint规则：`no-var`, jscs:`disallowVar`
    > 为什么？`let`是块作用域而不是像`var`一样是函数作用域
    ```
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
    ```
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
    ```
    // bad
    const item = new Object();

    // good
    const item = {};
    ```
-   当创建有动态属性的名称的对象时，使用计算属性
    > 为什么？这样允许你在一个地方定义此对象所有的属性
    ```
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
    ```
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
    ```
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
    ```
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker;

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
    ```
    // bad
    const bad = {
        'foo: 3,
        'bar: 4,
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
    ```
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
    ```
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