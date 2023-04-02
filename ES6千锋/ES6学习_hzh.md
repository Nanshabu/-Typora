ES6-13

![1667049967417](D:\!nanshabu\big3\MyLearningNotesBook\img\1667049967417.jpg)

#### let，var和const

2. let会报重复声明，var则比较随意，重不重复无所谓
      ```javascript
   // 使用 var 的时候重复声明变量是没问题的，只不过就是后面会把前面覆盖掉
   var num = 100
   var num = 200
   ```

   ```javascript
   // 使用 let 重复声明变量的时候就会报错了
   let num = 100
   let num = 200 // 这里就会报错了
   ```

   ```javascript
   // 使用 const 重复声明变量的时候就会报错
   const num = 100
   const num = 200 // 这里就会报错了
   ```
   
2. var对变量预解析可以“先使用再定义”，而let和const则不行，也就是没有变量提升

   ```javascript
   // 因为预解析（变量提升）的原因，在前面是有这个变量的，只不过没有赋值
   console.log(num) // undefined
   var num = 100
   ```
   ```javascript
   // 因为 let 不会进行预解析（变量提升），所以直接报错了
   console.log(num) 
   let num = 100
   ```
   ```javascript
   // 因为 const 不会进行预解析（变量提升），所以直接报错了
   console.log(num) 
   const num = 100
   ```

4. “let先使用再定义”（补充上一条）
   ![1667052203828](D:\!nanshabu\big3\MyLearningNotesBook\img\es6\1667052203828.jpg)
   ![1667052276905](D:\!nanshabu\big3\MyLearningNotesBook\img\es6\1667052276905.jpg)
   
4. var与window挂钩，会自动成为window的属性，let不会

      ```javascript
      var name = 'dasha'
      let age = 12
      console.log(window.name)	//	dasha
      console.log(window.age)		//	undefined
      ```

6. const定义常量，以上的都是叫变量，let和var都可以更改变量的值，const则不行
      ```javascript
      let num = 100
      num = 200
      console.log(num) // 200
      ```

      ```javascript
      const num = 100
      num = 200 // 这里就会报错了，因为 const 声明的变量值不可以改变（我们也叫做常量）
      ```

6. const声明时必须赋值，否则就会报错

      ```javascript
      let num
      num = 100
      console.log(num) // 100
      
      const num // 这里就会报错了，因为 const 声明的时候必须赋值
      ```

8. let和const都会被代码块限制作用范围，而var只有函数才能限制范围，其他的不能限制

     ```javascript
     // var 声明的变量只有函数能限制其作用域，其他的不能限制
     if (true) {
         var num = 100
         }
     console.log(num) // 100
     ```
     
     ```javascript
     // let 声明的变量，除了函数可以限制，所有的代码块都可以限制其作用域（if/while/for/...）
     if (true) {
         let num = 100
         console.log(num) // 100
     }
     console.log(num) // 报错
     
     // const 声明的变量，除了函数可以限制，所有的代码块都可以限制其作用域（if/while/for/...）
     if (true) {
         const num = 100
         console.log(num) // 100
     }
     console.log(num) // 报错
     ```

#### 解构赋值

通过解构赋值，可以快速从对象或者数组中取出属性或者数值。

object.freeze({})冻结对象的最初级属性，除非属性里面还有对象，冻结后无法改变对象的属性值

1. 解构赋值

   可以通过定位到数组或者对象的某一个位置，将值直接赋给一个或多个变量。

   ```js
   const arr = ['dasha', 'ersha', 'gangdan']
   let [a, b, c] = arr	//a='dasha' b='ersha' c='gangdan'
   //假如需要交换ab的值 只需要
   [a, b] = [b, a]
   ```

2. 解构赋值嵌套

   复杂的对象或者数组也可以使用这种方法。

   ```js
   const arr = [1, [2,3,4], 5]
   let [a, [b,,d], c] = arr
   
   console.log(a)	// 1
   console.log(b)	// 2
   console.log(d) 	// 4
   ```

3. 解构赋值的默认值

   给变量先设置好默认值，当数组或者对象中无法找到匹配的值，则将默认值赋给变量。

   ```js
   let [a=1] = [100] // a=100
   let [b=1] = [] // b=1
   ```

4. 解构赋值用在对象上时候，需要用键的方式

   ```js
   const obj = {
       name:'shabi',
       age:12,
   }
   let{age} = obj
   
   // 为了防止age在上面被let定义过了，可以将age改名为ag
   let{age:ag, err="定义err默认值即使对象中没有这个属性，也可以获取到这个默认值字符串"} = obj
   console.log(err)// '定义err默认值即使对象中没有这个属性，也可以获取到这个默认值字符串'
   ```

5. 解析一个从函数返回的数组

   获取返回值进行解构赋值，更加方便

   ```js
   function test(){
       return [1,2,3]
   }
   let [x,y] = test()
   console.log(x)	//x = 1
   console.log(y)	//y = 2
   ```

6. rest写法：将剩下的所有值赋值给一个变量

   这种写法只能适用于用在最后一位，无法用在开头或者中间，否则会报错。

   ```js
   let [a,...rest] = [1, 2, 3];
   console.log(a); // 1
   console.log(rest); // [2, 3]
   ```

#### 模板字符串

1. 不再使用字符串拼接，使用**模板字符串**，不用单双引号使用咱的反引号`

   使用${item}的方式填充变量

   ```js
   let num = 100
   let str = 'hello' + num + 'world' + num
   console.log(str) // hello100world100
   // 模版字符串拼接变量
   let num = 100
   let str = `hello${num}world${num}`
   console.log(str) // hello100world100
   ```

2. 模板字符串换行

   不会出现字符串换行需要加 \ 的毛病，随意换行书写

   ```js
   let s = "aaaaaa\
   aaaaaaaaaa\
   aaaa"
   let b = `aaaa
   aaaa
   aaaa`
   ```

3. 和map映射搭配可以生成简洁的代码(简洁的列表bushi)

   ```js
   let arr = ['dasha','ersha', 'sansha']
   let newlist = arr.map((item)=>{
       return `<li>${item}</li>`
       //console.log(`<li>${item}</li>`) 
   })
   console.log(newlist)
   ```

   ![1667120139482](D:\!nanshabu\big3\MyLearningNotesBook\img\ES6\1667120139482.jpg)

4. 模板字符串还支持三目运算符

   简直不要太简洁了~

   ```js
   let arr = ['dasha','ersha', 'sansha']
   let newlist = arr.map((item,index)=>{//将第一项li的样式添加active
       console.log(`<li class="${index==0?'active':''}">${item}</li>`) 
   })
   ```

字符串

1. string.includes | startsWith | endsWith

   - 是否包含
   - 是否开头
   - 是否结尾

2. string.repeat(int)
   字符串翻倍，一般也用不着。。

3. 支持二进制八进制十六进制写法
   0b100 0x100 0x100

4. Number .isFinite和.isNaN方法
   减少全局性方法，使得语言逐步模块化

   ```js
   let num1 = Number.isFinite(100) //true
   let num2 = Number.isFinite(100/0) //false
   let num3 = Number.isFinite(Infinity) // false
   let num4 = Number.isFinite("100") //false
   let num5 = isFinite("100")	//true自动转为数字再判断
   ```

   ```js
   let num1 = Number.isNaN(100) // false
   let num2 = Number.isNaN(NaN) //true
   let num3 = Number.isNaN("dasha") //false
   let num5 = isNan("dasha")	//true自动转为数字再判断  
   let num4 = Number.isNaN("100") // false
   ```

   它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。

5. isInteger方法\
   用来判断一个数值是否为整数。

   ```js
   let num1 = Number.isInteger(100) // true
   let num2 = Number.isInteger(100.0) //true
   let num3 = Number.isInteger("dasha") //false
   let num4 = Number.isInteger("100") // false
   ```

   

6. 极小常量Number.EPSILON
   它表示 1 与大于 1 的最小浮点数之间的差。2.220446049250313e-16

   ```js
   function isEqual(a,b){
           return Math.abs(a-b)<Number.EPSILON
   }
   
   console.log(isEqual(0.1+0.2,0.3)) //true
   console.log(0.1+0.2===0.3) //false
   ```

   

7. Math.trunc
   将小数部分抹掉,返回一个整数。

   ```js
   console.log(Math.trunc(1.2)) //1
   console.log(Math.trunc(1.8))// 1
   console.log(Math.trunc(-1.8)) //-1
   console.log(Math.trunc(-1.2))//-1
   ```

   

8. Math.sign
   `Math.sign`方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

9. 数组的拷贝使用扩展运算符（三点运算符）（都是浅复制）
   ES5：let arr2 = arr1.contact()
   ES6：let arr2 = [...arr1]

10. 数组合并（都是浅复制，深复制还不会）
    ES5：let arr3 = arr1.contact(arr2)
    ES6：let arr3 = [...arr1, ...arr2]

11. 数组的解构赋值
    let arr = [1,2,3,4,5] 
    let [a, b, ...c] = arr  // c: [3, 4, 5]

12. Array.from
    将类数组对象转换为真正数组

    ```js
    function test(){
            console.log(Array.from(arguments))
    }
    
    test(1,2,3)
    
    let oli = document.querySelectorAll("li")
    console.log(Array.from(oli))
    ```

    

13. Array.of 区别ES5的Array()
    将一组值转化为数组,即新建数组

    ```js
    let arr1 = Array(3)
    console.log(arr1)// [,,]
    
    let arr2 = Array.of(3)
    console.log(arr2)// [3]
    ```

    

14. find方法
    1)该方法主要应用于查找第一个符合条件的数组元素 

    2)它的参数是一个**回调函数**。在回调函数中可以写你要查找元素的条件,当条件成立为true时,返回该元素。如果没有符合条件的元素,返回值为undefined

    ```js
    let arr = [11,12,13,14,15]
    let res1 = arr.find(function(item){
        return item>13
    })
    let res2 = arr.findIndex(function(item){
        return item>13
    })
    console.log(res1) //14
    console.log(res2) //3
    ```

    22年ES13补充从后往前找的方法findLast和findLastIndex方法。

15. fill方法
    使用自己想要的参数替换原数组内容,但是会改变原来的数组

    ```js
    let arr1 = new Array(3).fill("dasha")
    let arr2 = ['a', 'b', 'c'].fill("dasha", 1, 2)
    console.log(arr1)//['dasha', 'dasha', 'dasha']
    console.log(arr2)// ['a', 'dasha', 'c']
    ```

    

16. 数组扁平化flat方法
    按照一个可指定的深度递归遍历数组,并将所有元素与遍历到的子数组中的元素合并为一个新数组返回

    ```js
    let arr = [11,12,[13,14,15]]
    console.log(arr.flat())	//[ 11, 12, 13, 14, 15 ]
    ```

    

17. 对象数组扁平化flatMap方法
    需要告诉解析器你要对对象里的哪个属性进行扁平化所以需要多写一个回调函数。

    ```js
    var obj = [{
                    name: "A",
                    list: ["鞍山", "安庆", "安阳"]
                },
                {
                    name: "B",
                    list: ["北京", "保定", "包头"]
                }
    ]
    console.log(obj.flatMap(item => item.list))
    ```

    

18. 通过扩展运算符合并的对象在出现同名属性时后者对象会覆盖前者对象

19. ==判断值是否相等，\=\=\=判断值和属性是否相等，Object.is和\=\=\=一样但是更强可以对NaN做判断了。

    ```js
    console.log(Object.is(NaN,NaN))//true
    console.log(NaN===NaN)//false
    console.log(NaN==NaN)//false
    ```

20. 参数设置默认值

    ```js
    function ajax(url, method="get", async=true){
        console.log(url,method,true)
    }
    ajax("/abc","get",true)	// /aaa get true
    ajax("/abc")	// /aaa get true
    ```

    不需要每次都传参了，有默认值少些很多多余的代码。

21. rest参数，参数也可以通过使用扩展运算符获取剩余参数

    ```js
    function test(...data){	//使用arguments的话不是获取到数组而是一个对象
        console.log(data)
    }
    test(1,2,3,4,5)	// [1,2,3,4,5]
    ```

    

22. 函数的name属性获取函数名

23. 箭头函数：简化写法 (  ) => {  }

24. 只有return 可以省略大括号，除非是返回一个对象，因为函数执行需要大括号，对象也是大括号包裹起来的，为了区分开来，就将函数执行的大括号改成了括号，而对象则是保留大括号的写法：(  )=>({obj})

25. 只有一个参数的话，参数的括号也可以去掉：arg=>{  }

26. 箭头函数也有缺点，无法访问arguments

27. 无法作为构造函数，new对象

28. 箭头函数没有this对象

29. symbol 新的原始数据类型，表示独一无二的值

30. 不能进行运算

31. 不能转换数据，除了boolean

32. symbol更多是作为一种书写规范吧，对于以后开发可能会少但是某一些工具里可能会有至少需要认识一下。

33. symbol放对象不能正常遍历

34. `Object.Object.getOwnPropertySymbols`获取对象中的所有symbol属性

35. `Reflat.ownKeys(obj)`获取对象中所有属性和symbol属性

36. symbol也可以当作常量，赋予它应有的功能和用途

37. Symbol.for()可以重新使用同一个 Symbol 值

    ```js
    var obj  ={
        [Symbol.for("name")]:"dasha",
        [Symbol.for("age")]:100
    }
    
    console.log(obj[Symbol.for("name")])
    ```

    

38. 迭代器

    ```js
    let arr = ["dasha", "ersha", "sansha"]
    
    for(let i of arr){
        console.log(i)
    }	// dasha  ersha  sansha
    ```

    数组中内置的迭代器以及迭代器方法

    ```js
    let arr = ['a','b','c']
    let i = arr[Symbol.iterator]()
    console.log(i.next())	//{ value: 'a', done: false }
    console.log(i.next())	//{ value: 'b', done: false }
    console.log(i.next())	//{ value: 'c', done: false }
    console.log(i.next())	//{ value: undefined, done: true }
    ```

    > Iterator 的遍历过程是这样的。
    >
    > （1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
    >
    > （2）第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
    >
    > （3）第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
    >
    > （4）不断调用指针对象的next方法，直到它指向数据结构的结束位置。
    >
    > ES6 规定，默认的 Iterator 接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）。Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。

39. 原生默认具备 Iterator 接口的数据结构如下。

    - Array
    - Set
    - Map
    - String
    - arguments 对象
    - NodeList 对象

    自定义对象没办法使用迭代器，但是可以通过拟化一个类似数组的对象进行迭代器遍历，例：

    ```js
    let obj = {
        0: "dasha",
        1: "ersha",
        2: "sansha",
        length: 3,
        [Symbol.iterator]: Array.prototype[Symbol.iterator]
    }
    for(let i of obj){
        console.log(i)
    }
    ```


#### Set结构

set类似于数组，但是内部成员不能有相同的值，一般被用作去重复值

```js
let arr = [...set1]
console.log(arr)	//Set(4) { 1, 2, 3, 4 }
```

输出结果显然不是数组，但是可以通过迭代器转化为数组

```js
let arr = [...set1]
console.log(set1)	//[ 1, 2, 3, 4 ]
console.log(Array.from(set1))
```

1. set的add方法

   直接将值添加到set的末尾

   ```js
   let set2 = new Set()
   set2.add(1)
   set2.add(2)
   set2.add(3)
   console.log(set2)	//Set(3) { 1, 2, 3 }
   
   //也可以通过链式的结构添加
   set2.add(4).add(5)	//Set(5) { 1, 2, 3, 4, 5 }
   ```

2. set的size属性

   ```js
   let set3 = new Set()
   console.log(set3.size)
   ```

3. 判断set中是否含有某个值has方法

   ```js
   console.log(set2.has(100))	//false
   console.log(set2.has(1))	//true
   ```

   

4. 删除某个值delete方法

   ```js
   set1.delete(1)
   ```

5. 清空所有值

   ```js
   set1.clear()
   ```

6. set的键值和键名一样都是成员的值

7. 数组可以通过遍历entries获得索引和键值

   set可以通过forEach方法遍历获得索引和键值（不过这两者也是相等的）

   ```js
   Set.forEach((item, index)=>{
   	console.log(item, index)
   })
   ```

8. 对某一个复杂数组进行去重

   ```js
   let arr = [1, 2, 3, "data", {name: "dasha"}, {name: "dasha"},[1, 2],[3, 4],[3, 4]]
   
   function unique(list) {
       let res = new Set()
       //设置过滤器，对每一个元素进行筛查，若res中没有则添加到res中以进行下一次判断并返回true,非true会被自动过滤掉。
       return list.filter(item => {
           //转为字符串，才能排除对象作为键名的成员
           let id = JSON.stringify(item)
           if (res.has(id)) {
               return false
           } else {
               res.add(id)
               return true
           }
       })
   }
   
   console.log(unique(arr))	//[ 1, 2, 3, 'data', { name: 'dasha' }, [ 1, 2 ], [ 3, 4 ] ]
   ```

#### Map结构

map类似于对象，都是以键值对的形式，但是他的键名可以更丰富，不止是字符串、数值，甚至可以用对象来当作键名。也内置了迭代器

1. 初始化Map

   ```js
   let map1 = new Map([["name","nsb"], ["age", 15], ["home", "canton"], [{x: 99}, "xbox"]])
   
   console.log(map1)
   ```

   

2. 添加键值对set方法

   ```js
   let map2 = new Map()
   map2.set("name","nsb")
   map2.set({x:1},"奥里给")
   
   console.log(map2)
   ```

   

3. 内置迭代器可以letof遍历，输出的每一项都是数组，0是键，1是值

4. set、get、has、delete、clear和set一样（`set.add( ), map.set( )`）

5. keys()、values()、entries()、forof、foreach都是和set类似的

#### proxy对象

代理对象，作为对象和对象属性中间的代理人，当你获取这个对象属性或者修改这个对象属性以及实例化的一些操作，都会触发到代理对象，我们就可以通过代理对象在这一过程中进行一系列自定义的操作。

1. proxy的初始化
2. proxy的get方法
3. proxy的set方法
4. proxy的has方法
5. proxy的this指向问题

#### Reflect对象

抽象概念，一直使用Object来调用内部方法老是觉得怪怪的，所以官方就添加了Reflect对象供开发者使用。用以代替Object来调用方法。

1. 使用defineProperty方法
2. 修改某些Object方法返回结果
3. has方法和deleteProperty方法让js更接近 函数行为
4. 搭配proxy使用，get和set方法内部定义使用。

#### Promise对象

ES6为了避免回调地狱的出现，推行使用Promise对象进行函数的回调，这是ES6中的一大特性，接收一个function函数参数为resolve和reject，成功了调用resolve失败了调用reject

1. promise对象的状态：pending、fulfilled、rejected
2. 状态发生变化，就不会再有新的状态变化，最终结果只有两种。
3. promise的链式回调
4. Promise.all( )
5. Promise.race( )

#### Generator对象

1. 初始化方法
2. 迭代器和yield的使用
3. 搭配promise使用形成一个链块，将异步请求写成同步请求的写法但是需要手动操作。
4. g.next取到gen对象，.value取到promise对象，.then进行回调函数，.next()穿结果给下一个请求。

#### class类对象

1. 不再使用function来定义一个 新对象，使用class使逻辑更加严谨
2. 创建的对象和类所指向的原型对象是相同的，全等为true
3. 内部自带get、set方法可随时用来给某个属性作监听,一般不对该属性直接进行修改，通过这个属性改变其他属性。
4. 静态属性，对类的属性直接进行赋值，创建的实例不会获得这个属性，只有类自己才有的属性或者方法。简单说就是不需要实例化的属性或者方法。
5. 类的继承，通过super调用父类
6. 用面向对象的思想完成页面渲染父类子类都上场。

#### 模块化Moudle

1. script标签异步加载	async和defer，defer会在页面渲染完后加载

2. 隐式规范加个下划线 _a 防君子不防小人

3. `type="module"`让script标签模块化

   异步加载 

4. 导出导入模块

   `import A1 from './1.js'`

   `explore default A1`

5. 在哪使用就必须引入模块

6. 多种写法需要注意

7. ES6模块（简称ESM）和CommonJS模块（简称CJS）

   CJS是Node.js专用的，与ES6模块不兼容，导入导出语法也有所差异。

#### Nodejs的模块化

1. 默认不支持ES6写法，使用CJS的写法

2. 通过生成package.json 添加type属性设置为`type: "module"`支持ES6写法

   在当前文件下的控制台输入`npm init`疯狂回车之后生成package.json文件，在文件中添加`type: "module"`即可

3. 将文件后缀js改为mjs也可以支持ES6写法

#### ES7新特性

1. Math对象的求幂运算符 `Math.pow()` 

   ```js
   Math.pow(3, 2) === 3 ** 2    // 9
   ```

2. 针对数组的include方法，实现判断数组中是否含有某个值得功能

3. indexOf方法，同样针对数组，检查该值在数组中的索引位置

#### async和await！

async和await需要同时使用，缺一不可，目的是让异步函数能够跟主线函数同步，等待异步函数完成再进行下一步。

1. async返回值是 Promise。
2. `await`命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。
3. 可以和trycatch搭配使用，遇到错误时即执行catch
4. 也可以搭配`Promise.all([])`使用

#### 对象方法扩展

1. Object.keys(obj)取出所有的键

2. Object.values(obj)取出所有的值

3. Object.entries(obj)取出所有的键值，二维数组

   搭配new Map(entries)方法生成map

4. Object.getOwnPeropertyDescriptors(obj)获取对象所有属性的描述符

5. 使用Object.assign复制对象无法复制getset方法

   `Object.defineProperties()`和`Object.getOwnPropertyDescriptors()`搭配使用完成对象的所有复制

#### ES8-字符串填充

1. str.padStart()

   在字符串从前面将长度补满

   

2. str.padEnd()

   在字符串从后面将长度补满

3. 使用padStart完成数字补零

#### ES8-函数参数结尾可以加逗号

1. 有用但不完全有用

#### ES9-rest和扩展运算符

1. rest写法

   ```js
   let obj = {
       name: "大傻",
       age: 21,
       location: "东北",
   }
   
   let {name, ...other} = obj
   
   function test({name, ...other}){
       console.log(name, other) 
   }
   test(obj)	// 大傻 { age: 21, location: '东北' }
   ```

2. 对object的扩展运算符...

   将对象或者数组展开，可以用于两个对象或者数组的合并，遵循后来者居上。

   ```js
   let obj = {
       name: "大傻",
       age: 21,
       location: "东北",
   }
   let obj1 = {
       name: "二傻",
       age: 18,
       location: "东北",
   }
   let obj2 = {...obj, ...obj1}
   console.log(obj2)		//{ name: '二傻', age: 18, location: '东北' }
   ```

   也可以用作浅复制（浅拷贝）

#### ES9-正则扩展

1. 对正则表达捕获到的结果进行命名

   ```js
   let str = "今天是2022-11-09"
   
   let reg = /(?<year>[0-9]{4})-(?<mouth>[0-9]{2})-(?<day>[0-9]{2})/
   
   let res = reg.exec(str)
   console.log(res)
   /* 输出结果为
   0: "2022-11-09"
   1: "2022"
   2: "11"
   3: "09"
   groups: {year: '2022', mouth: '11', day: '09'}
   index: 3
   input: "今天是2022-11-09"
   */
   ```

#### ES9-Promise.finally

是指除了then和catch之后必须执行的代码，跟java的trycatchfinal一样。

1. 

#### ES9-异步迭代（P31 有点看不懂

1. 异步生成器 async function *generator( )

2. 异步遍历器 for-await-of

3. 同步遍历器无法异步遍历异步生成器

   遍历输出后结果仍然无法异步执行

   ```js
           async function test(){
               let arr = [timer(1000),timer(1000),timer(1000),]
               for (const iterator of arr) {
                   console.log(await iterator)
               }
           }
           test()
   ```

   

4. 使用异步遍历器遍历异步生成器

   异步遍历器遍历异步生成器（自带遍历器）使用`for-await-of`

   ```js
           //使用异步遍历器遍历异步生成器
           function timer(t) {
               return new Promise(resolve => {
                   setTimeout(() => {
                       resolve(t)
                   }, t)
               })
           }
   
   
           async function* fn() {
               yield timer(1000)//任务1
               yield timer(3000)//任务2
               yield timer(2000)//任务3
           }
   
   
           // 使用一下 for await ...of
           async function fn1() {
               for await (const val of fn()) {
                   console.log("start", Date.now())
                   console.log(val);
                   console.log("end", Date.now())
               }
           }
           fn1();
   ```

   

5. 案例-nodejs读取文件

   ```js
   // 传统写法
   let fileName = 'file.txt'
   const fs = require("fs");
   function main(inputFilePath) {
     const readStream = fs.createReadStream(
       inputFilePath,
       { encoding: 'utf8', highWaterMark: 1024 }
     );
     readStream.on('data', (chunk) => {
       console.log('>>> '+chunk);
     });
     readStream.on('end', () => {
       console.log('### DONE ###');
     });
   }
   main(fileName)
   
   // 异步遍历器写法
   let fileName = 'file.txt'
   const fs = require("fs");
   
   async function main(inputFilePath) {
     const readStream = fs.createReadStream(
       inputFilePath,
       { encoding: 'utf8', highWaterMark: 1024 }
     );
   
     for await (const chunk of readStream) {
       console.log('>>> '+chunk);
     }
     console.log('### DONE ###');
   }
   main(fileName)
   ```

   

#### ES10-Object.fromEntries()

将数组，集合转化为对象的方法。

```js
const arr = [["name", "kerwin"], ["age", 100]];
console.log(Object.fromEntries(arr))//{name: 'kerwin', age: 100}

const m = new Map()
m.set("name","tiechui")
m.set("age",18)
console.log(Object.fromEntries(m))
```

1. 用处1

   搭配`URLSearchParams()`方法，将地址栏参数转为对象

   ```js
   let str ="name=kerwin&age=100"
   
   let searchParams = new URLSearchParams(str)
   console.log(Object.fromEntries(searchParams))
   //{name: 'kerwin', age: '100'}
   ```

#### ES10-trimStart()和trimEnd()

去掉字符串前面或者后面的空格。

#### ES11-Promise.allSettled()

1. 区别于之前的Promise.all方法

   前者在遇到错误后，会直接走catch分支，不管正确错误的都走错误的。后者则无论如何都走then分支，错误的请求也会同成功的放到同一个数组中。

2. 在渲染页面时可能会遇到请求多个接口，必定会出现有请求错误的情况，所以不能一棒子打死，使用allSettled方法可以避免一棒子的情况。

#### ES11-module新增

1. 动态导入import()

   当需要时才加载对应的模块，做到按需编译，减少多余模块的加载以提高性能。

   import()返回一个promise对象，可以搭配await使用，使代码更简洁

2. import.meta

   返回一个对象，包含该模块的url路径，仅限于模块内部使用

3.  export * as obj from 'module'

   export搭配as使用，多用于在B模块需要继承A模块的情况下，不需要复制一遍代码，直接在原有基础上便可以使用A模块。


#### ES11-String的matchAll方法

通过以前的学习，我们了解到正则表达式和正则表达式的匹配，如for循环搭配str.match(reg)或者reg.exec(str)，后者居多。

新增的matchAll方法更多是偏向于String类，算是match方法的一种增强吧。该方法会返回一个迭代器，包含匹配该正则表达式的值。

```js
let str = `
<ul>
<li>1111</li>
<li>2222</li>
<li>3333</li>
<li>4444</li>
</ul>
`
let reg = /<li>(.*)<\/li>/g

  let object = str.matchAll(reg)
  for (const iterator of object) {
    console.log(iterator)
  }
```

输出结果：

```
[
  '<li>1111</li>',
  '1111',
  index: 6,
  input: '\n<ul>\n<li>1111</li>\n<li>2222</li>\n<li>3333</li>\n<li>4444</li>\n</ul>\n',
  groups: undefined
]
[
  '<li>2222</li>',
  '2222',
  index: 20,
  input: '\n<ul>\n<li>1111</li>\n<li>2222</li>\n<li>3333</li>\n<li>4444</li>\n</ul>\n',
  groups: undefined
]
[
  '<li>3333</li>',
  '3333',
  index: 34,
  input: '\n<ul>\n<li>1111</li>\n<li>2222</li>\n<li>3333</li>\n<li>4444</li>\n</ul>\n',
  groups: undefined
]
[
  '<li>4444</li>',
  '4444',
  index: 48,
  input: '\n<ul>\n<li>1111</li>\n<li>2222</li>\n<li>3333</li>\n<li>4444</li>\n</ul>\n',
  groups: undefined
]
```

#### ES11-BigInt

1. BigInt的写法

   简简单单，在数字后面加个n就是bigint了

2. BigInt的属性和算法

   使用typeOf结果为bigInt，双等号number结果为true，三等号number结果为false。

   bigInt加减乘除都需要和同类型否则会报错，用来作数值比较的话，就不局限于同类型。

3. BigInt()方法

   类型强转为BigInt

4. 使用场景

   前后端交互数据的时候难免遇到一些贼大的id值，这时候就需要双方协调统一数据。

#### ES11-globalThis

区分在不同环境下的最顶层对象，比如浏览器就是window，在node里面就是global，self就是self，globalThis在什么环境就是什么对象。

#### ES11-空值合并运算符??

两个值，a??b，若a为空b有值，则返回b，区别于||，对于0等false来说||是行不通的，但是控制运算符??是可以通过的。

```js
console.log(undefined??1);
console.log(null??1);
console.log(false??1);
console.log(NaN??1);
console.log(''??1);
1
1
false
NaN
 

console.log(undefined||1);
console.log(null||1);
console.log(false||1);
console.log(NaN||1);
console.log(''||1);
1
1
1
1
1
```

#### ES11-可选链操作符?.

可选链操作符，假如有那就链，这样子避免访问一个undifined报错。

```js
let obj = {
    name:"dasha",
    // location:{
    //     href:"canton"
    // }
}

console.log(obj?.location?.href);
//undifined
```

#### ES12-新增逻辑操作符

类似+=，-=的写法让逻辑运算同理，&&=，||=，??=

#### ES12-数字分隔符

let a = 1_000_000 ，单单为了方便程序员看

#### ES12-replaceAll方法

str.replaceAll("any","replaceWord")

#### ES12-Promise.any

和Promise.race方法很像，但是不会因为某个Promise变成reject就结束，必须等到有某一个Promise变成resolve才会停下，否则就catch

```js
ajax1 = ()=>{
    return new Promise((resovle,reject)=>{
        reject("ajax1")
    })
}
ajax2 = ()=>{
    return new Promise((resovle,reject)=>{
      reject("ajax2")
    })
}
ajax3 = ()=>{
    return new Promise((resovle,reject)=>{
      reject("ajax3")
    })
}

Promise.any([ajax1(),ajax2(),ajax3()]).then((res)=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
```

输出结果：AggregateError: All promises were rejected

#### ES12-weakSet、weakMap、weakRef

针对垃圾清除机制，在set中无法针对某一对象进行清空，这种情况下就得使用weakSet，不过代价就是无法保存基本数据类型，不存在引用计数+1，size和for也不能用

1. 
2. 
3. 