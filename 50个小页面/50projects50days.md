# 50projects50days

学完了前端三件套包括ajax，对于前端的知识进行一个梳理和实战，所以就那这个经典的五十个项目实战来操练操练。这个是五十个前端项目的官方地址：[50个前端实战项目](http://50projects50days.com/)

我自己也是上传到了github上进行展示：!nanshabu.github.io/50projects50days

使用github部署静态网页还是很cool的，具体可以查看:[如何在github上部署静态网页](http://t.zoukankan.com/jiaoshou-p-13420031.html)

### 一、Expanding-cards伸缩卡片

<img src="D:\!nanshabu\big3\MyLearningNotesBook\img\expanding-cards.jpg" alt="expanding-cards" style="zoom:50%;" />

核心知识点：通过动画transition的形式改变子元素flex属性在父元素container中的占比来实现卡片的特写展示

1. #### 居中标准

   ```css
     /* flex盒子 */
     display: flex;
     /* 垂直居中 */
     align-items: center;
     /* 水平居中 */
     justify-content: center;
   ```

   ##### flex弹性盒子

   这篇文章详细解答了弹性盒子的所有内容：[display-flex的详细解答](https://blog.csdn.net/idealistic/article/details/79955806)

   - flex-direction{主轴方向}
   - flex-wrap{是否换行}
   - flex-flow{flex-direction加flex-wrap}
   - justify-content{排布方式}
   - align-items{对齐方式}
   - align-content{根据轴线的对齐方式}

   需要注意的是：

   > 比较好用的就是居中的设置，除此之外还可以通过设置**flex-direction**来设置主轴的方向[默认值row横轴 | row-reverse反向横轴 | column竖轴 | column-reverse反向竖轴]
   >
   > flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

2. #### 设置body为100vh和overflow: hidden

   ```css
     height: 100vh;
     /* 100vh一般会有个进度条给他overflow hidden一下 */
     overflow: hidden;
   ```

   - vh: 相对于视窗的高度, 视窗被均分为100单位的vh;100vh相当于百分之百的视窗高度
   - vw: 相对于视窗的宽度, 视窗被均分为100单位的vw;100vw相当于百分之百的视窗宽度

3. #### 将一个图片完整的塞入一个盒子

   ```css
   background-size: cover;
   ```

4. #### 透明度属性opacity

   value范围0-1

5. #### 让字体保持不换行

   ```css
   white-space: nowrap;
   ```

   当窗口发生改变时，文本默认会随着窗口大小变化而变化，这时就需要使用white-space: nowrap。**细节决定了你的页面的高级感。**

### 二、progress-steps进度步骤

<img src="D:\!nanshabu\big3\MyLearningNotesBook\img\project1_screen_shot.jpg" alt="project1_screen_shot" style="zoom: 50%;" />

实现原理：设置伪类元素为空进度条，创建单独的控制条，通过改变该元素的width实现进度条的递进或递减。同时需要判断点亮多少个路标让prev和next该处于什么状态。

1. #### ::before有什么作用？::before与:before的区别是什么？
   

::before和::after等这种形式的都被称作为伪类元素。

   [::before有什么作用？::before与:before的区别是什么？-css教程-PHP中文网](https://www.php.cn/css-tutorial-412795.html)

   你可以正常的设置伪类元素的属性，其与跟随的元素是互不影响的。

   1、伪类元素要配合content属性一起使用

   2、伪类元素是css渲染层加入的，不能通过js来操作

   3、伪类对象特效通常通过：hover伪类样式来激活

2. #### CSS中定义变量

   定义全局变量

   ```css
   :root{
       --bgcolor: #ccc;
   }
   ```

   定义某元素下的变量

   ```css
   .element{
       --bgcolor: #ccc;
   }
   ```

3. #### box-sizing: border-box

   默认为content-box当你设置好了该盒子的宽度和高度后，调整了padding和border的数值，content-box会帮你在原宽度和高度上为盒子调整padding和border，使得盒子总宽度和总高度为三者之和。

   当你把box-sizing改为border-box（怪异盒子）后，当你再次设置盒子的宽度和高度时，此时的宽度和高度就是把padding和border算在里面的总宽度和总高度了，之后你再怎么调padding和border的数值，盒子的总宽度和总高度都是保持不变。

4. #### transform: translate(-50%,-50%)!!!

   [(3条消息) 详解transform:translate(-50%,-50%)_汤圆真的好可爱的博客-CSDN博客_transform translate](https://blog.csdn.net/qq_41083105/article/details/115233510)

5. #### transition: all 0.5s ease 0.3s

   - transition-property
   - transition-duration
   - transition-timing-function
   - transition-delay

   all表示所有的properties, 0.5表示持续时间, ease表示变化方式, 0.3s表示延迟的时间

6. #### 使用querySelector读取null的属性"addEventListener"会null报错

   如果你想对某一个元素添加事件的话，优先选择getElementById，如果使用querySelector会导致报空指针错误。

### 三、rotating-nav-animation

<img src="D:\!nanshabu\big3\MyLearningNotesBook\img\rotating-nav-animation.jpg" alt="rotating-nav-animation" style="zoom: 50%;" />

1. #### <nav>元素

    并不是所有的 HTML 文档都要使用到 <nav> 元素。<nav> 元素只是作为标注一个导航链接的区域。在不同设备上（手机或者PC）可以制定导航链接是否显示，以适应不同屏幕的需求。可以理解为前端开发的一个书写规范。

2. #### linear：规定以相同的速度开始至结束的过渡效果。
   
   ease：规定慢速开始逐渐变快然后慢速结束的过渡效果。
   ease-in：规定以慢速度开始的过渡效果。
   ease-out：规定以慢速度结束的过渡效果。
ease-in-out：规定以慢速开始至结束的过渡效果。
   
3. #### 水一篇文章：CSS中的rotate旋转

    本节的难点，需要了解transform-origin的设置，设置旋转角度transform: rotate( x deg)

4. #### 垂直水平居中的好办法

   ```css
     top: 50%;
     left: 50%;
     transform: translate(-50%,-50%);
   ```

5. #### 从坐标轴开始向右移动+0px 

    transform: translateX(0px);

6. ####  消除默认点击蓝色边框效果，搭配border: 0使按钮特效及边框消失

    outline: none;

    PS: 该方法也可用于input输入框

7. #### ES6写法

   为元素添加单击事件的ES6写法，看起来整洁多了。

   ```javascript
   const open = document.getElementById('open')
   const container = document.querySelector('.container')
   open.addEventListener('click',() => container.classList.add('show-nav'))
   ```

8. #### 背景改为和父元素的一样，相当于透明了

    background: transparent;

9. #### 设置旋转基点，设置为左上角

    transform-origin: top left;

### 四、Hidden-search

<img src="D:\!nanshabu\big3\MyLearningNotesBook\img\hidden-search.jpg" alt="hidden-search" style="zoom:50%;" />

核心攻克：通过调整position: absoluted使得button可以覆盖在input上，之后对输入框的width做调整以及按钮的位置做调整即可。

1. #### 背景渐变色

   通过调整deg来调整渐变的角度。

   ```css
   background-image: linear-gradient(90deg,green,blue);
   /* 两种以上的颜色 */
   background-image: linear-gradient(90deg, white 0%, red 50%, #000 100%);
   ```

   调整占比来调整渐变程度。当百分占比相等时则表现为无渐变。

   ```css
   background-image: linear-gradient(45deg,#87f 60%,#f78 60%);
   ```

   ![a8fd81381c4a8da249ceed26aedd7260](D:\!nanshabu\big3\MyLearningNotesBook\img\a8fd81381c4a8da249ceed26aedd7260.png)

   除此之外，linear-gradient可以用于渐变色或者在图片做覆盖渐变

   ```css
   background:linear-gradient(250deg,#a0a 30%,rgba(0,0,0,0) 90%),url('XXX.png');
   ```

   <img src="D:\!nanshabu\big3\MyLearningNotesBook\img\linear-gradient.jpg" alt="linear-gradient" style="zoom:50%;" />


### 五、Blurry-roading

1. #### calc()的使用

   通过calc调整section的位置避免白边的模糊出现，试着删除这句width: calc(100vw + 50px)改成width: 100vw会看到明显的白边。这句话简单可以理解为是对图片的放大避开白边的出现。
   
   ```css
width | height: calc(100vh|100vw + 10px) 
   ```

   ##### calc() 函数用于动态计算长度值。
   
   - 需要注意的是，运算符前后都需要保留一个空格，例如：`width: calc(100% - 10px)`；
   - 任何长度值都可以使用calc()函数进行计算；
- calc()函数支持 "+", "-", "*", "/" 运算；
   - calc()函数使用标准的数学运算优先级规则；

   ##### 自动调整表单域的大小以适应其容器的大小

   **calc()** 的另外一个用例是用来确保一个表单域的大小适合当前的可用空间，而不会在保持合适的外边距的同时，因挤压**超出**其容器的边缘。

   以下实例中，form 元素自身使用了窗口可用宽度的 1/6，然后，为了让 form 元素内部的 input 元素保持合适的大小，我们再一次使用了 calc()，让它的宽度为其容器的宽度减 1em。
   
   ```css
   input {
     padding: 2px;
     display: block;
     width: calc(100% - 1em);
   }
    
   #formbox {
     width: calc(100% / 6);
     border: 1px solid black;
     padding: 4px;
}
   ```
   
   2.setInterval的使用方法
   
   ```js
   let int = setInterval(function, time);
   clearInterval(int);
   ```
   
   需要搭配clearInterval()使用，没通过time时间就执行一次function方法，当你需要停止时则使用clearInterval(int)

### 六、scroll-animation

![scroll-animation](D:\!nanshabu\big3\MyLearningNotesBook\img\scroll-animation.gif)

1. css-复习一下flex盒子的居中方法

   ```css
       display: flex;
       align-items: center;
       justify-content: center;
   ```

2. css-flex盒子通过flex-direction来控制摆放方向

   ```css
   flex-direction: column;
   ```

   flex盒子的具体属性可以参考[Day1Expanding-cards伸缩卡片]('www.baidu.com')

3. css-添加阴影

   `   box-shadow: 10px 10px 0px 50px rgba(0,0,0,0.5);`

   box-shadow六个参数，分别为**offset-x offset-y blur spread color position;**对应横轴 纵轴 扩散度 阴影内外位置（这里不选为外阴影内阴影为inset）

4. js-添加scroll事件

   ```js
   window.addEventListener('scroll', function)
   ```

5. js-获取元素到达顶部的距离（核心功能实现）

   ```css
   box.getBoundingClientRect().top
   ```

   关于getBoundingClientRect方法的详细介绍可看[getBoundingClientRect方法介绍]('www.baidu.com')

   getBoundingClientRect方法用于获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置。

   <img src="D:\!nanshabu\big3\MyLearningNotesBook\img\1666667749575.jpg" alt="1666667749575" style="zoom: 67%;" />

   通过比对box.getBoundingClientRect().top和window.innerHeight来判断盒子是否可以弹出，再将此方法加入到scroll事件中，即可实现本话功能。

### 七、Split Landing Page

![split landing page](D:\!nanshabu\big3\MyLearningNotesBook\img\split landing page.gif)

1. css-声明属性

   ```css
   :root {
     --left-bg-color: rgba(87, 84, 236, 0.7);
     --right-bg-color: rgba(43, 43, 43, 0.8);
     --left-btn-hover-color: rgba(87, 84, 236, 1);
     --right-btn-hover-color: rgba(28, 122, 28, 1);
     --hover-width: 75%;
     --other-width: 25%;
     --speed: 1000ms;
   }white-space:nowarap
   ```

2. css-弹性盒子居中

   ```css
     display: flex;
     justify-content: center;
     align-items: center;
     transform: translate(-50%, -50%);
   ```

   这里不要忘了设置transform。

3. css-调整盒子的高和宽记得检查子元素是否会溢出父元素，使用`overflow: hidden`隐藏起来

4. css-背景图片自动适应盒子大小

   ```css
     background-repeat: no-repeat;
     background-size: cover;
   ```

5. css-借用::before对元素添加滤镜

   ```css
   .right::before{
     content: '';
     width: 100%;
     height: 100%;
     position: absolute;
     background-color: var(--right-bg-color);
   }
   ```

   设置绝对定位防止::before给子元素让位使得滤镜无法完全覆盖

6. js-箭头函数的使用

   ```js
   left.addEventListener('mouseenter', () => container.classList.add('hover-left'))
   left.addEventListener('mouseleave', () => container.classList.remove('hover-left'))
   
   right.addEventListener('mouseenter', () => container.classList.add('hover-right'))
   right.addEventListener('mouseleave', () => container.classList.remove('hover-right'))
   ```

   `fn = (val) =>{}`就相当于`function fn(val){}`

   ```js
   fn = (val) =>{return 'hello'}
   function fn(val){return 'hello'}
   ```

   也可以用作匿名函数的写法例如例子中的

   ```js
   right.addEventListener('mouseleave', () => container.classList.remove('hover-right'))
   ```

   需要注意的是箭头函数的`this`，因为箭头函数没有对`this`的绑定，对于箭头函数，`this`关键字始终表示定义箭头函数的对象(window)。而常规函数的`this`始终指向调用该函数的对象。
   可以查看w3c的具体解释[JavaScript 箭头函数 (w3school.com.cn)](https://www.w3school.com.cn/js/js_arrow_function.asp)

### 八、form-input-wave

<img src="D:\!nanshabu\big3\MyLearningNotesBook\img\form-input-wave.gif" alt="form-input-wave" style="zoom:80%;" />

1. css-使用padding来实现居中

   ```css
   padding: 20px 40px;
   ```

   假如这里仍然使用弹性盒子的特性来实现居中的话，需要额外设置父元素的宽高，计算每个子元素需要预留多少位置，因为有一个跳动的文字，需要为这个文字提前预留位置。所以优先使用padding。

2. css-在父元素中使用绝对定位

   对子元素设置`position:absolute`，对父元素设置`position:relative`，如果没有设置后者的话，子元素会在body里面进行定位。这里需要对跳动的文字`<label>`设置为absolute，但是一开始没有给form-control设置为relative，就会出现很尴尬的画面。

3. css-transparent

   ```css
     background-color: transparent;
   ```

   设置背景颜色和父元素的一样，看起来像透明的一样耶( •̀ ω •́ )y

4. ccs-单独设置bottom

   虽然不常用但还是蛮常用的。

   ```css
     border-bottom: 2px #fff solid;
   ```

5. ccs-取消默认特效

   ```css
   outline: 0;
   ```

   可用于按钮和输入文本框。

6. css-三次贝塞尔曲线cube-bezier()

   ```css
     /* cube-bezier() 函数定义三次贝塞尔曲线 */
     transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
   ```

   可能以后也用不着吧，当作课外知识+1

   看看w3c的演示[CSS cubic-bezier() 函数 (w3school.com.cn)](https://www.w3school.com.cn/cssref/func_cubic-bezier.asp)

7. css-input:require

   `<input name="pwd" type="password" required>`

   不输入这个文本框无法提交表单

8. js-代码部分

```js
const labels = document.querySelectorAll('.form-control label')
labels.forEach(label => {
    label.innerHTML = label.innerText
        .split('')
        .map((letter, idx) => `<span style="transition-delay:${idx * 50}ms">${letter}</span>`)
        .join('')
})
```

代码虽短，但对入门还是值得好好琢磨琢磨，通过分解单词遍历每一个字母，在对字母设置不同的过渡延迟时间，形成字母跳动的效果。涉及了字符串的分割拼接，map的使用。

### 九、Sound Board

![1666919826316](D:\!nanshabu\big3\MyLearningNotesBook\img\1666919826316.jpg)

点击各个按钮会发出不一样很尴尬的声音😅，但重点肯定不是这上面，咱看一下html代码先。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>Sound Board</title>
  </head>
  <body>
    <audio id="applause" src="sounds/applause.mp3"></audio>
    <audio id="boo" src="sounds/boo.mp3"></audio>
    <audio id="gasp" src="sounds/gasp.mp3"></audio>
    <audio id="tada" src="sounds/tada.mp3"></audio>
    <audio id="victory" src="sounds/victory.mp3"></audio>
    <audio id="wrong" src="sounds/wrong.mp3"></audio>

    <div id="buttons"></div>

    <script src="script.js"></script>
  </body>
</html>

```

看得出来，点击按钮后播放对应的audio即可。但可以看到button只有一个元素，且是div，不难猜出这部分应该主要是学习js中的一些方法。

简单看一下css的内容

```css
* {
  box-sizing: border-box;
}

body {
  background-color: rgb(161, 100, 223);
  font-family: 'Poppins', sans-serif;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  text-align: center;
  height: 100vh;
  overflow: hidden;
  margin: 0;
}

.btn {
  background-color: rebeccapurple;
  border-radius: 5px;
  border: none;
  color: #fff;
  margin: 1rem;
  padding: 1.5rem 3rem;
  font-size: 1.2rem;
  font-family: inherit;
  cursor: pointer;
}

.btn:hover {
  opacity: 0.9;
}

.btn:focus {
  outline: none;
}

```

重点放在js文件上

```js
const sounds = ['applause', 'boo', 'gasp', 'tada', 'victory', 'wrong']
const buttons = document.getElementById('buttons')

sounds.forEach(sound => {
    // document.createElement('')创建新的元素
    const button = document.createElement('button')
    button.classList.add('btn')
    button.innerText = sound
    button.addEventListener('click',()=>{
        // 点击按钮时先停止在播的audio（小细节）
        stopSongs()
        document.getElementById(sound).play()
    })
    // .appendChild()方法添加新元素(对象)
    buttons.appendChild(button)
});
// 封装停歌的方法
function stopSongs(){
    sounds.forEach(sound=>{
        const song = document.getElementById(sound)
        song.pause()
    })
}
```

1. js-创建新的元素（这里是button）

   documen.createElement('元素名')，把元素创建放在js文件中，能够提高代码的整洁和增强代码的逻辑性。这一句是配合foreach使用遍历数组同时创建对应的按钮。

   ```js
   const button = document.createElement('button')
   ```

2. js-添加新的子元素

   appendElement(对象)，上面咱们对button对象添加了class：`button.classList.add('btn')`添加了文本内容：`button.innerText = sound`，也添加完了监听事件：

   ```js
   button.addEventListener('click',()=>{
       // 点击按钮时先停止在播的audio（小细节）
       stopSongs()
       document.getElementById(sound).play()
   })
   ```
   就可以直接将button对象添加成为div buttons的子元素了，经过数组的遍历后将所有button添加到buttons下，完成按钮的生成😍

3. js-媒体文件的暂停和开始

   pause() | play()  <audio controls>可以查看完整的音视频控件

   <audio id="applause" src="sounds/applause.mp3" controls></audio>

   ![1666923999381](D:\!nanshabu\big3\MyLearningNotesBook\img\1666923999381.jpg)

### 十、Dad Jokes

![1667957995549](D:\!nanshabu\big3\MyLearningNotesBook\img\1667957995549.jpg)

1. box-sizing: border-box;

   将border和padding的数值计算包含在盒子的width和height中，这样做的好处是修改padding或者border后盒子的大小保持不变

   ```css
   * {
     box-sizing: border-box;
   }
   ```

   

2. [css padding 报错：invalid property value](https://segmentfault.com/q/1010000021882855)

3. async和await

   ```js
   async function generateJoke(){
     let config = {
           headers: {
             Accept: 'application/json',
           },
     }
   
     let res = await fetch('https://icanhazdadjoke.com',config)
   }
   ```

4. fetch请求

   [JavaScript中Fetch() 的用法示例（代码）-js教程-PHP中文网](https://www.php.cn/js-tutorial-413140.html)

5. res.json()

   https://wenku.baidu.com/view/0d24597b383567ec102de2bd960590c69ec3d8c7.html?_wkts_=1667957305104&bdQuery=res.json%28%29%E6%96%B9%E6%B3%95

   [fetch 拿到的数据为 ReadableStream 格式的处理方式](https://www.jianshu.com/p/48d5e3150c9e)

6. 中文笑话接口：'https://autumnfish.cn/api/joke/list?num=1"

7. 'application/json'

   [application/json-常见的post提交数据的方式](https://blog.csdn.net/a1782519342/article/details/124980975)


### 十一、Event Keycodes

