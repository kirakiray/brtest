# brtest

为BrowserRequire开发的单元测试工具

## 已确认兼容性

移动端全线兼容；

winPC >=IE8(best:>=ie10)

其他全线兼容

## 使用方法

### 1.引入依赖文件

```html
<link rel="stylesheet" type="text/css" href="brtest.css" />
<script src="brtest.js"></script>
```

### 2.添加测试组

#### 2.1 对等测试
```javascript
brtest.test('对等测试标题',function(con){
    con.equal('对等测试1',1,1);
    //因为 1 == 1 为true，显示为绿色
    
    con.equal('对等测试2',1,2);
    //因为 1 == 2 为false，显示为红色
    
    con.more('匹配测试',1,[1,2,3]);
    //因为 1 与后面数组内的某个值（1）相等，所以显示绿色
    
    con.more('匹配测试2',1,[4,2,3]);
    //因为 1 与后面数组内都不相等，所以显示红色
});
```

如果对等测试内容为object，在全等的情况下才会显示绿色；如果是克隆对象，则会显示黄色警告状；

#### 2.2 顺序测试

```javascript
brtest.orderTest('顺序测试标题',function(con){
    //设定顺序
    con.set('one');
    con.set('two');
    con.set('three');

    con.order('one');
    //one属于第一级，显示绿色
    
    con.order('three');
    //three属于第三级，在one（第一级）后面，所以显示绿色
    
    con.order('two');
    //two属于第二级，因为第三级已经触发，所以显示红色
});
```

顺序测试还能设置触发次数限制，count可设置触发固定次数 

```javascript
brtest.orderTest('顺序测试标题2',function(con){
    //设定顺序
    con.count('one',2);
    //设置one必须触发两次

    con.order('one');
    //第一次触发one，次数不足两次，显示为警告黄色
    
    con.order('one');
    //第二次触发one，次数两次显示为绿色
    
    con.order('one');
    //第三次触发one，次数超过两次显示为红色
    
    con.count('two',2,4);
    //设置two触发2到4次
    
    con.order('two');
    //第一次触发two为警告黄色 
    
    con.order('two');
    //第二次触发two为满足次数，显示绿色     
    
    con.order('two');
    //第三次触发two为满足次数，显示绿色 
    
    con.count('three',2,true);
    //设置three触发次数大于等于2次
    
    con.order('three');
    //第一次触发three为未满次数，显示黄色  
    
    con.order('three');
    //第二次触发three为满足次数，显示绿色 
    
    con.order('three');
    //第三次触发three为满足次数，显示绿色 
});
```

顺序测试还能设置同等触发权限

```javascript
brtest.orderTest('顺序测试标题3',function(con){
    //设定顺序
    con.set('one').set('two');
    con.set('three');

    con.order('two');
    //two与one属于同一级触发顺序，所以为绿色
    
    con.order('one');
    //two与one属于同一级触发顺序，所以为绿色
    
    con.order('two');
    //two与one属于同一级触发顺序，所以为绿色
    
    con.order('three');
    //three属于第二级触发顺序，所以为绿色
});
```

关于细致的特性，可以查看 brtestExample.html 测试文件

#### 2.3 顺序测试（全局）

顺序测试可以进行全局触发

```javascript
brtest.orderTest('orderTest1',function(con){
    //设定顺序
    con.set('one')
    con.set('two');
    con.set('three');
});

brtest.order('orderTest1',"one");
//one属于第一级，显示绿色

brtest.order('orderTest1',"three");
//three属于第三级，在one（第一级）后面，所以显示绿色

brtest.order('orderTest1',"two");
//two属于第二级，因为第三级已经触发，所以显示红色
```

