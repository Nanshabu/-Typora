ES6-13

![1667049967417](D:\nanshabu\big3\MyLearningNotesBook\img\1667049967417.jpg)

1. let和var在代码块中的区别：
   let只存在于代码块中，var则存在全局
   
2. 重复声明？let: var =>报错

3. var先使用在定义let不行

4. 暂存性死区补充上一条
   ![1667052203828](D:\nanshabu\big3\MyLearningNotesBook\img\es6\1667052203828.jpg)
   ![1667052276905](D:\nanshabu\big3\MyLearningNotesBook\img\es6\1667052276905.jpg)

5. var与window挂钩，自动成为window的属性，let不会

6. const定义常量，以上的都是叫变量

7. 和let一样不能重复不能跳出块级以及暂存性死区以及不与window顶层对象挂钩

8. object.freeze({})冻结对象的最初级属性，除非属性里面还有对象，冻结后无法改变对象的属性值

9. 解构赋值

   ```js
   const arr = ['kerwin', 'tiechui', 'gangdan']
   let [a, b, c] = arr
   //假如需要交换ab的值 只需要
   [a, b] = [b, a]
   ```

10. 解构赋值嵌套

    ```js
    const arr = [1, [2,3,4], 5]
    let [a, [b,,d], c] = arr
    //假如需要交换ab的值 只需要
    console.log(a)	// 1
    console.log(b)	// 2
    console.log(d) 	// 4
    ```

11. 解构赋值的默认值

    ```js
    let [a=1] = [100] // a=100
    let [b=1] = [] // b=1
    ```

12. 解构赋值用在对象上也可以只不过要用键的方式

    ```js
    const obj = Object.freeze({
        name:'shabi',
        age:12,
    })
    let{age} = obj
    
    // 为了防止age在上面被let定义过了，所以可以改更高级的写法
    //假如age被let过了那么就会起名为ag，除非ag也被定义过了😅
    let{age:ag, err="定义err默认值即使对象中没有这个属性，也可以获取到这个默认值字符串"} = obj
    console.log(err)// '定义err默认值即使对象中没有这个属性，也可以获取到这个默认值字符串'
    ```

13. 解构水很深，多尝试解构取出你想要的值

14. 赋值结构很常用，在项目中经常会使用到，可以使你的传参更加高效，代码也会更加整洁

15. 不再使用字符串拼接，使用**模板字符串**，不用单双引号使用咱的反引号`

    ```js
    let num = 100
    let str = 'hello' + num + 'world' + num
    console.log(str) // hello100world100
    // 模版字符串拼接变量
    let num = 100
    let str = `hello${num}world${num}`
    console.log(str) // hello100world100
    ```

    

16. 模板字符串换行不会出现字符串换行需要加 \ 的毛病，随意换行书写

17. 和map映射搭配可以生成简洁的代码

    ```js
    let arr = ['dasha','ersha', 'sansha']
    let newlist = arr.map((item)=>{
        return `<li>${item}</li>`
        //console.log(`<li>${item}</li>`) 
    })
    console.log(newlist)
    ```

    ![1667120139482](D:\nanshabu\big3\MyLearningNotesBook\ES6\img\1667120139482.jpg)

18. 模板字符串还支持三目运算符

    ```js
    let arr = ['dasha','ersha', 'sansha']
    let newlist = arr.map((item,index)=>{//将第一项li的样式添加active
        console.log(`<li class="${index==0?'active':''}">${item}</li>`) 
    })
    ```

    

19. string.includes | startsWith | endsWith

    - 是否包含
    - 是否开头
    - 是否结尾

20. string.repeat(int)
    字符串翻倍，一般也用不着。。

21. 支持二进制八进制十六进制写法
    0b100 0x100 0x100

22. Number .isFinite和.isNaN方法
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
    let num3 = Number.isNaN("kerwin") //false
    let num5 = isNan("kerwin")	//true自动转为数字再判断  
    let num4 = Number.isNaN("100") // false
    ```

    它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。

23. isInteger方法\
    用来判断一个数值是否为整数。

    ```js
    let num1 = Number.isInteger(100) // true
    let num2 = Number.isInteger(100.0) //true
    let num3 = Number.isInteger("kerwin") //false
    let num4 = Number.isInteger("100") // false
    ```

    

24. 极小常量Number.EPSILON
    它表示 1 与大于 1 的最小浮点数之间的差。2.220446049250313e-16

    ```js
    function isEqual(a,b){
            return Math.abs(a-b)<Number.EPSILON
    }
    
    console.log(isEqual(0.1+0.2,0.3)) //true
    console.log(0.1+0.2===0.3) //false
    ```

    

25. Math.trunc
    将小数部分抹掉,返回一个整数。

    ```js
    console.log(Math.trunc(1.2)) //1
    console.log(Math.trunc(1.8))// 1
    console.log(Math.trunc(-1.8)) //-1
    console.log(Math.trunc(-1.2))//-1
    ```

    

26. Math.sign
    `Math.sign`方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

27. 数组的拷贝使用扩展运算符（三点运算符）（都是浅复制）
    ES5：let arr2 = arr1.contact()
    ES6：let arr2 = [...arr1]

28. 数组合并（都是浅复制，深复制还不会）
    ES5：let arr3 = arr1.contact(arr2)
    ES6：let arr3 = [...arr1, ...arr2]

29. 数组的解构赋值
    let arr = [1,2,3,4,5] 
    let [a, b, ...c] = arr  // c: [3, 4, 5]

30. Array.from
    将类数组对象转换为真正数组

    ```js
    function test(){
            console.log(Array.from(arguments))
    }
    
    test(1,2,3)
    
    let oli = document.querySelectorAll("li")
    console.log(Array.from(oli))
    ```

    

31.  Array.of 区别ES5的Array()
    将一组值转化为数组,即新建数组

    ```js
    let arr1 = Array(3)
    console.log(arr1)// [,,]
    
    let arr2 = Array.of(3)
    console.log(arr2)// [3]
    ```

    

32. find方法
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

33. fill方法
    使用自己想要的参数替换原数组内容,但是会改变原来的数组

    ```js
    let arr1 = new Array(3).fill("kerwin")
    let arr2 = ['a', 'b', 'c'].fill("kerwin", 1, 2)
    console.log(arr1)//['kerwin', 'kerwin', 'kerwin']
    console.log(arr2)// ['a', 'kerwin', 'c']
    ```

    

34. 数组扁平化flat方法
    按照一个可指定的深度递归遍历数组,并将所有元素与遍历到的子数组中的元素合并为一个新数组返回

    ```js
    let arr = [11,12,[13,14,15]]
    console.log(arr.flat())	//[ 11, 12, 13, 14, 15 ]
    ```

    

35. 对象数组扁平化flatMap方法
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

    

36. 通过扩展运算符合并的对象在出现同名属性时后者对象会覆盖前者对象

37. ==判断值是否相等，\=\=\=判断值和属性是否相等，Object.is和\=\=\=一样但是更强可以对NaN做判断了。

    ```js
    console.log(Object.is(NaN,NaN))//true
    console.log(NaN===NaN)//false
    console.log(NaN==NaN)//false
    ```

38. 参数设置默认值

    ```js
    function ajax(url, method="get", async=true){
        console.log(url,method,true)
    }
    ajax("/abc","get",true)	// /aaa get true
    ajax("/abc")	// /aaa get true
    ```

    不需要每次都传参了，有默认值少些很多多余的代码。

39. rest参数，参数也可以通过使用扩展运算符获取剩余参数

    ```js
    function test(...data){	//使用arguments的话不是获取到数组而是一个对象
        console.log(data)
    }
    test(1,2,3,4,5)	// [1,2,3,4,5]
    ```

    

40. 函数的name属性获取函数名

41. 箭头函数：简化写法 (  ) => {  }

42. 只有return 可以省略大括号

43. 