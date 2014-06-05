# JavaScript Guide

#### 变量

声明变量必须加上 var 关键字.

Decision:
当你没有写 var, 变量就会暴露在全局上下文中, 这样很可能会和现有变量冲突. 另外, 如果没有加上, 很难明确该变量的作用域是什么, 变量也很可能像在局部作用域中, 很轻易地泄漏到 Document 或者 Window 中, 所以务必用 var 去声明变量.

#### 常量

常量的形式如: NAMES_LIKE_THIS, 即使用大写字符, 并用下划线分隔. 你也可用 @const 标记来指明它是一个常量. 但请永远不要使用 const 关键词.

Decision:

对于基本类型的常量, 只需转换命名.

```javascript
    /**
     * The number of seconds in a minute.
     * @type {number}
     */
    SECONDS_IN_A_MINUTE = 60;
```

对于非基本类型, 使用 @const 标记.

```javascript
    /**
     * The number of seconds in each of the given units.
     * @type {Object.<number>}
     * @const
     */
    SECONDS_TABLE = {
      minute: 60,
      hour: 60 * 60,
      day: 60 * 60 * 24
    }
```

这标记告诉编译器它是常量.

至于关键词 const, 因为 IE 不能识别, 所以不要使用.

#### 分号

总是使用分号.
如果仅依靠语句间的隐式分隔, 有时会很麻烦. 你自己更能清楚哪里是语句的起止.

#### 闭包

可以, 但小心使用.
闭包也许是 JS 中最有用的特性了. 有一份比较好的介绍闭包原理的文档.

有一点需要牢记, 闭包保留了一个指向它封闭作用域的指针, 所以, 在给 DOM 元素附加闭包时, 很可能会产生循环引用, 进一步导致内存泄漏. 比如下面的代码:

```javascript
    function foo(element, a, b) {
      element.onclick = function() { /* uses a and b */ };
    }
```

这里, 即使没有使用 element, 闭包也保留了 element, a 和 b 的引用, . 由于 element 也保留了对闭包的引用, 这就产生了循环引用, 这就不能被 GC 回收. 这种情况下, 可将代码重构为:

```javascript
    function foo(element, a, b) {
      element.onclick = bar(a, b);
    }

    function bar(a, b) {
      return function() { /* uses a and b */ }
    }
```

#### eval()

不要使用

#### with() {}

不要使用

#### 数组和对象的初始化

如果初始值不是很长, 就保持写在单行上:

```javascript
    var arr = [1, 2, 3];  // No space after [ or before ].
    var obj = {a: 1, b: 2, c: 3};  // No space after { or before }.
```

初始值占用多行时, 缩进2个空格.

```javascript
    // Object initializer.
    var inset = {
      top: 10,
      right: 20,
      bottom: 15,
      left: 12
    };

    // Array initializer.
    this.rows_ = [
      '"Slartibartfast" <fjordmaster@magrathea.com>',
      '"Zaphod Beeblebrox" <theprez@universe.gov>',
      '"Ford Prefect" <ford@theguide.com>',
      '"Arthur Dent" <has.no.tea@gmail.com>',
      '"Marvin the Paranoid Android" <marv@googlemail.com>',
      'the.mice@magrathea.com'
    ];

    // Used in a method call.
    goog.dom.createDom(goog.dom.TagName.DIV, {
      id: 'foo',
      className: 'some-css-class',
      style: 'display:none'
    }, 'Hello, world!');
```

比较长的标识符或者数值, `不要`为了让代码好看些而手工对齐. 如:

```javascript
    CORRECT_Object.prototype = {
      a: 0,
      b: 1,
      lengthyName: 2
    };
    不要这样做:

    WRONG_Object.prototype = {
      a          : 0,
      b          : 1,
      lengthyName: 2
    };
```

#### 函数参数

尽量让函数参数在同一行上. 如果一行超过 80 字符, 每个参数独占一行, 并以4个空格缩进, 或者与括号对齐, 以提高可读性. 尽可能不要让每行超过80个字符. 比如下面这样:

```javascript
    // Four-space, wrap at 80.  Works with very long function names, survives
    // renaming without reindenting, low on space.
    goog.foo.bar.doThingThatIsVeryDifficultToExplain = function(
        veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
        tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
      // ...
    };

    // Four-space, one argument per line.  Works with long function names,
    // survives renaming, and emphasizes each argument.
    goog.foo.bar.doThingThatIsVeryDifficultToExplain = function(
        veryDescriptiveArgumentNumberOne,
        veryDescriptiveArgumentTwo,
        tableModelEventHandlerProxy,
        artichokeDescriptorAdapterIterator) {
      // ...
    };

    // Parenthesis-aligned indentation, wrap at 80.  Visually groups arguments,
    // low on space.
    function foo(veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
                 tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
      // ...
    }

    // Parenthesis-aligned, one argument per line.  Visually groups and
    // emphasizes each individual argument.
    function bar(veryDescriptiveArgumentNumberOne,
                 veryDescriptiveArgumentTwo,
                 tableModelEventHandlerProxy,
                 artichokeDescriptorAdapterIterator) {
      // ...
    }
```

#### 传递匿名函数

如果参数中有匿名函数, 函数体从调用该函数的左边开始缩进2个空格, 而不是从 function 这个关键字开始. 这让匿名函数更加易读 (不要增加很多没必要的缩进让函数体显示在屏幕的右侧).

```javascript
    var names = items.map(function(item) {
                            return item.name;
                          });

    prefix.something.reallyLongFunctionName('whatever', function(a1, a2) {
      if (a1.equals(a2)) {
        someOtherLongFunctionName(a1);
      } else {
        andNowForSomethingCompletelyDifferent(a2.parrot);
      }
    });
```

#### 更多的缩进

事实上, 除了 初始化数组和对象 , 和传递匿名函数外, 所有被拆开的多行文本要么选择与之前的表达式左对齐, 要么以4个(而不是2个)空格作为一缩进层次.

```javascript
    someWonderfulHtml = '' +
                        getEvenMoreHtml(someReallyInterestingValues, moreValues,
                                        evenMoreParams, 'a duck', true, 72,
                                        slightlyMoreMonkeys(0xfff)) +
                        '';

    thisIsAVeryLongVariableName =
        hereIsAnEvenLongerOtherFunctionNameThatWillNotFitOnPrevLine();

    thisIsAVeryLongVariableName = 'expressionPartOne' + someMethodThatIsLong() +
        thisIsAnEvenLongerOtherFunctionNameThatCannotBeIndentedMore();

    someValue = this.foo(
        shortArg,
        'Some really long string arg - this is a pretty common case, actually.',
        shorty2,
        this.bar());

    if (searchableCollection(allYourStuff).contains(theStuffYouWant) &&
        !ambientNotification.isActive() && (client.isAmbientSupported() ||
                                            client.alwaysTryAmbientAnyways()) {
      ambientNotification.activate();
    }
```

#### 空行

使用空行来划分一组逻辑上相关联的代码片段.

```javascript
    doSomethingTo(x);
    doSomethingElseTo(x);
    andThen(x);

    nowDoSomethingWith(y);

    andNowWith(z);
```

#### 二元和三元操作符

操作符始终跟随着前行, 这样就不用顾虑分号的隐式插入问题. 如果一行实在放不下, 还是按照上述的缩进风格来换行.

```javascript
    var x = a ? b : c;  // All on one line if it will fit.

    // Indentation +4 is OK.
    var y = a ?
        longButSimpleOperandB : longButSimpleOperandC;

    // Indenting to the line position of the first operand is also OK.
    var z = a ?
            moreComplicatedB :
            moreComplicatedC;
```

#### 字符串

使用 ' 优于 "
单引号 (') 优于双引号 ("). 当你创建一个包含 HTML 代码的字符串时就知道它的好处了.

```javascript
    var msg = 'This is some HTML';
```

> Google JavaScript 编码风格原文,
