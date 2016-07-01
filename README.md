# srtest 2.0

It is a modular test framework for SugarRequire

写给 SugarRequire 用的测试框架。

## 关于srtest

由于BrowserRequire升级2.0版本改名为SugarRequire，所以为它做的测试框架brtest升级也要更名srtest啦哈哈。

当然不止更名，相关的使用方法也会变得更加自由更方便，抛弃brtest的callback形式的设定，才用面向对象的方式设定测试程序，更加直接方便。

框架依赖jquery，以后会有剥离jquery的版本。

## 已确认兼容性

移动端全线兼容；

winPC >=IE8，ie8及以下版本需要依赖jquery-1.9.1；

其他全线兼容

## 使用方法

### 引入依赖文件

```html
<link rel="stylesheet" type="text/css" href="srtest.css" />
<script src="jquery-2.2.0.js"></script>
<script src="brtest.js"></script>
```

创建一个测试组视图的方法，arguments[0] 就是组标题

```javascript
//用于创建测试组
var g1 = srtest.group('我是第一组');

//打印
g1.log('我是g1 普通 信息');
g1.warn('我是g1 警告性 信息');
g1.error('我是g1 错误 信息');
g1.succeed('我是g1 成功 信息');
```

### 重建信息

用于修改已经打印的信息。

```javascript
var log1 = g1.warn('我还没执行');

//一秒后改变状态
setTimeout(function(){
    log1.succeed('我执行成功了');
},1000);
```

### 执行次数记录测试

次数测试常用来记录事件的触发次数，异步函数的执行次数；

通过orderTest 可以制定数目测试；

```javascript
g1.setOrder({
    name : "test1",
    count : 1
});
```

在设定之后，通过order来触发

```javascript
g1.order('test1');
```

上面案例设定了 name为test1的命令，而且test1必须**只**触发 1 次才算正确（安全提示）；不足1次则警告状态，超过1次则出现错误状态；（因为执行0次有可能会正确，但是超过1次是绝对错误）

当然，我们也能设定次数区间，方便记录次数。

```javascript
g1.setOrder({
    name : "test1",
    descript : "我是描述出来的文字",
    max : 3,
    min : 1
});
```

这个案例在执行1次以下是警告状态；1至3次内是安全提示；超过3次是错误提示；

### 执行顺序和次数记录测试

顺序测试常用于测试事件的触发先后顺序，异步函数的执行顺序，也会伴随着次数的记录；

通过orderTest也可以制定顺序规则；

```javascript
g1.setOrder({
    name : "test1",
    count : 1
},{
    name : "test2",
    count : 1
});
```

上面案例设定了name为test1和test2的命令，当test1被执行1次后，在执行1次test2才能安全提示；

当然也能将`count`替换`max`和`min`；