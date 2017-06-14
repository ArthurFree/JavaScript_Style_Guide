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

