


# ES6引入外部模块分两种情况：

1.导入外部的变量或函数等；

```
import {firstName, lastName, year} from './profile';

```

2.导入外部的模块，并立即执行

```
import './test'         // 执行test.js，但不导入任何变量

```

## 两种情况区别

第2种情况就不用讲了，就是执行从头到尾执行引入的js文件，当然，会忽略js文件里export。

第1种情况：

``` a.js

console.log('111111');
export var a = 'aaaaaa';
console.log('222222');
```


``` b.js

import {a} from './a.js';
console.log('333333');
console.log(a);

```

运行结果：

```
111111
222222
333333
aaaaaa
```

和预期一样

但是：：：：

b.js 把console.log(a)这句去掉，看看运行结果

```
333333
```

咦，只输出这一句了？我们明明引入了 a 模块了，为何模块里的代码没有执行呢

这要从ES6模块加载的实质谈起



## import 的实质

在阮一峰老师的ES6入门一书里讲到：

遇到模块加载命令import时，不会去执行模块，而是只生成一个动态的只读引用。

等到真的需要用到时，再到模块里面去取值，换句话说，ES6的输入有点像Unix系统的“符号连接”，原始值变了，import输入的值也会跟着变



可是按照上面说的，那有console.log(a)的时候，应该是输出3333，1111，2222的顺序，而不是1111,2222,333的顺序呀。


我猜测是上面讲的不细致的原因，应该分为编译时和运行时来说。

ES6的这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，在编译的时候，发现了后面有使用到a的地方，就在运行时遇到import的地方直接执行了模块的代码。当然只是自己的猜测。


