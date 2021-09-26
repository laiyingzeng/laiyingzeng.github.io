## 一、Lexical Environment

> A Lexical Environment is a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code.

一个Lexical Environment包含**一个Environment Record和一个outer Lexical Environment reference**。其中Environment Record记录变量绑定信息。

**Environment Records：**

在这个规范中有两种主要的Environment Record值：Declarative Environment Records 和 Object Environment Records。Environment Records各种值的关系可以认为是一个简单的面向对象层次结构，Environment Record是一个抽象类，它有三个具体的子类： declarative Environment Record, object Environment Record 和 global Environment Record（其实global Environment Record封装了一个declarative Environment Record和一个object Environment Record）。Function Environment Records 和 module Environment Records 是 declarative Environment Record 的子类。

**注意：**

ES2021中没有了Lexical Environment概念，直接是Environment Record，指向外部环境的outer Lexical Environment reference也变为outer Environment Record reference，保存在Environment Record的[[OuterEnv]]属性中。Function Objects的[[Environment]]值也变为Environment Record。

Lexical Environment和Environment Record都纯粹是规范机制，不需要对应于ECMAScript实现的任何特定工件。ECMAScript程序不可能直接访问或操作这些值。

## 二、Execution Contexts

> An execution context is a specification device that is used to track the runtime evaluation of code by an ECMAScript implementation.

任何时候最多只有一个执行上下文在实际执行代码，这被称为正在运行的执行上下文。一个栈（即**执行上下文栈**）被用于跟踪执行上下文，正在运行的执行上下文总是这个栈的顶部元素。执行上下文包含跟踪相关代码的执行进度所需的特定状态。包括这些state components：code evaluation state，Function，Realm，LexicalEnvironment，VariableEnvironment。

**主要讲讲LexicalEnvironment，VariableEnvironment：**

（这里以ES6为例，注意ES5中会有些不同，如ES5中VariableEnvironment不仅包括VariableStatements还有FunctionDeclarations）

<img src="http://203.195.243.98:8888/notebook/1Front-end/Web/JavaScript/Advanced/image1.png"/>

执行上下文的LexicalEnvironment和VariableEnvironment组件总是Lexical Environments。当创建一个执行上下文时，它的LexicalEnvironment和VariableEnvironment组件最初具有相同的值。

**注意：**

执行上下文纯粹是一种规范机制，不需要对应于ECMAScript实现的任何特定工件。ECMAScript代码不可能直接访问或观察执行上下文。

## 三、关于Function Objects的[[Environment]]

ES5时是[[Scope]]，且只有Function Objects内部属性有[[Scope]]，值类型为Lexical Environment。

ES6后改为[[Environment]]，且不止Function Objects有这个属性，Script Records和Module Records也有这个属性。在ES2021前值类型也为Lexical Environment。

Function Objects的[[Environment]]是在FunctionInitialize (F, kind, ParameterList, Body, Scope)调用时设置，为传入的**Scope**，这个操作是在初始化Function Object。并且之后只在NewFunctionEnvironment ( F, newTarget )调用时有用到这个值，即在创建一个新的Function Environment时用到，作为该Lexical Environment的the outer Lexical environment reference。（这里以ES6为例，ES5中也是相同的阶段，只不过叫做Creating Function Objects和Entering Function Code）

- FunctionInitialize (F, kind, ParameterList, Body, Scope)https://262.ecma-international.org/6.0/#sec-functioninitialize
- NewFunctionEnvironment ( F, newTarget )https://262.ecma-international.org/6.0/#sec-newfunctionenvironment



**参考：**

- [ES5规范](https://262.ecma-international.org/5.1/)
- [ES6规范](https://262.ecma-international.org/6.0/)

