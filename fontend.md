# 前端的学习三维度
1. 立足标准，搭建自己的知识体系。
2. 立足团队，从业务和工程的角度思考前端的价值和发展需要。
3. 学会软件开发工程师的通用编程能力和架构能力。


# 前端知识体系
1. [基础模块1：HTML+CSS](#1-1)
2. [基础模块2：JavaScript](#1-2)
3. [基础模块3：宿主及其API](#1-3)
4. [实践部分：基于团队的综合运用](#1-4)


# <a id="1-1">HTML+CSS</a>
   ## HTML
   1. 元素
      1. 文档元信息
         1. title
         2. meta
         3. style
         4. link
         5. base
      2. 语义相关内容
         1. section
         2. nav
      3. 链接
      4. 替换型元素
         1. img
         2. video
         3. audio
      5. 表单
         1. input
         2. button
      6. 表格
      7. 总集
   2. 语言
      1. 实体
      2. 命名空间
   3. 补充标准
      1. AIRA
   ## CSS
   1. 语言
      1. @rule
      2. 选择器
      3. 单位
   2. 功能
      1. 布局
         1. 正常流
         2. 弹性布局
      2. 绘制
         1. 颜色和形状
         2. 文字相关
      3. 交互
         1. 动画
         2. 其他交互

# <a id="1-2">JavaScript</a>
   1. 文法（面向developer）
      1. 词法
      2. 语法
   2. [运行时（面向执行环境）](#runtime)
      
   3. 语义：developer通过文法，把**语义**传递给运行时。

# <a id="1-3">宿主及其API</a>
   1. 浏览器
      1. 实现原理
         1. 解析
         2. 构建DOM树
         3. 计算CSS
         4. 渲染
         5. 合成
         6. 绘制
      2. api
         1. BOM
         2. DOM
         3. CSSOM
         4. 事件
         5. api总集合
   2. node

# <a id="1-4">基于团队的综合运用</a>
   1. 性能
      1. 方法论
      2. 技术体系
   2. [工具链](#manage-4)
      1. 建设思路
   3. [持续集成](#manage-3)
   4. 搭建系统
      1. 定义
      2. 常见类型
   5. 架构与基础库
      1. 前端架构的主要任务
         1. 兼容
         2. 复用
         3. 能力扩展

# 技术管理的基础
   1. 性能
   2. 发布
   3. <a id="manage-3">持续集成</a>
   4. <a id="manage-4">工具链</a>

# <a id="runtime">JavaScript运行时</a>
1. 数据结构
   1. 类型
      1. 8种基本类型
         1. 值类型
            1. null
               1. 关键字，不会被覆盖为其它值，可以放心使用
               2. [null和undefined的区别](#diffNullUndefined)
            2. undefined
               1. 表示：未定义，没人管。
               2. 任何刚声明的变量都是Undefined类型，值为undefined
               3. undefined不是关键字，容易篡改，建议用**void 0**获取undefined的值
               4. [null和undefined的区别](#diffNullUndefined)
            3. boolean
            4. number
               1. 大致对应数学的有理数，表示2^64 - 2^53 +3个值
               2. 特殊值
                  1. NaN：2^53-2
                  2. Infinity
                  3. -Infinity
                  4. +0 & -0
                     1. 除法领域要特别注意
                     2. 检测方法 1/x 是不是 Infinity
               3. 能表示的整数范围：-0x1fffffffffffff 到 0x1fffffffffffff
               4. 处理小数时不能使用==或者===：
                  1. Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON
            5. string
               1. 对Unicode使用UTF16编码
               2. 编码后长度限制：2^53 - 1
               3. 值类型
               4. BMP:码点值 0-65536（U+0000 - U+FFFF）
                  1. 表示超出BMP范围外的码点时要格外小心
               5. 操作字符串要格外小心，建议try...catch...
            6. Symbol：对象中非字符串key的集合
               1. es6的对象系统被Symbol重塑
               2. 创建方式：Symbol();
               3. 使用Symbol.iterator定义for of
                  ```
                     var o = new Object

                     o[Symbol.iterator] = function() {
                        var v = 0
                        return {
                              next: function() {
                                 return { value: v++, done: v > 10 }
                              }
                        }        
                     };

                     for(var v of o) 
                        console.log(v); // 0 1 2 3 ... 9 
                  ```
            7. bigint
         2. 引用类型
            1. object
               1. 对象是属性的集合
               2. 属性：key(字符串或者Symbol)-value
                  1. 访问器属性
                  2. 数据属性
               3. 类：运行时对象的一个私有属性
               4. .运算符提供了装箱操作，会生成一个临时对象
               5. 类型转换
                  1. string转number
                     1. parseInt：在不传入第二个参数的情况下，parseInt 只支持 16 进制前缀“0x”，而且会忽略非数字字符，也不支持科学计数法。
                     2. parseFloat： 则直接把原字符串作为十进制来解析，它不会引入任何的其他进制。
                     3. Number 是比 parseInt 和 parseFloat 更好的选择
                  2. number转string
                     1. 极大或极小的数字会转为科学计数法```0.0000000000000000000000005.toString() // 5e-25```
                  3. 装箱
                     1. 装箱转换会产生大量临时对象，应该在性能要求较高的场景下避免使用装箱操作。
                  4. 如何判断一个变量是不是数字（而不是Number的对象）```Object.prototype.toString.call(num)===[object number] && typeof num === 'number'```
               6. 如何创建非Object类型的实例
                  ```const temp = Object.create(null)
                     console.log(temp instanceof Object); // false
                  ```
      2. 7中语言类型
         1. List 和 Record
         2. Set
         3. Completion Record
         4. Reference
         5. Property Descriptor
         6. Lexical Environment 和 Environment Record
         7. Data Block
2. 实例（内置对象）
3. 算法
   1. 事件循环
   2. 微任务执行
   3. 函数的执行
   4. 语句级的执行

# <a id="diffNullUndefined">undefined和null的区别</a>
1. undefined表示没人管，不是明确表示这个值为空
   1. 写代码过程中，一般不会给变量赋值为undefined，这样可以保证所有值为undefined都是从未赋值的自然状态。
2. null表示明确表示，这个值是空值

# TIPS
1. 一家公司吸引人才的能力能看出他未来的发展趋势。winter从微软盛大去了当时名不见经传的淘宝，说明淘宝可以吸引人才。司徒正美及ymfe团队可以留在qunar也说明，qunar是吸引人才的，后来qunar衰落他们随即离开，也印证了qunar的衰败。ymfe团队离开qunar，去了美团。截止到今天（2020年03月26日09:16:15），美团的市值已经仅次于阿里和腾讯，除了在本地生活和阿里系分庭抗礼，还要进军电商。  
2. 前端的工程理论，来自于服务端、客户端，比如：持续集成、线上监控、前后端分离
3. 为了人工作久了技术进步会很缓慢？因为进入了舒适区，比如擅长做css，就一直在做css。说白了，就是吃老本。你的地位，来自于你在某一方面的优势，因为不愿意放弃自己的地位，所以拒绝在自己欠缺的方面尝试。
4. 目前值得入坑的前端社区
   1. npm
   2. github
5. 教程推荐
   1. https://www.theodinproject.com/home
   2. frontend master
   3. odin project
   4. codepen上的trick
6. 前端知识图谱需要更新，需要传播，需要完备，便于查阅。还需要一个字段，用于表征他是否被点亮，以表示是否我们使用过，或者记录使用次数，证明熟练度。
7. 学习方法
   1. 0基础:《高程3》、《精通css》、[mdn](https://developer.mozilla.org/zh-CN/)
   2. 1年经验：看重学前端，掌握学习方法、梳理知识架构、理解技术思想。
   3. 建立渐进式的知识架构
      1. 顶级目录依赖逻辑，保持完备性
      2. 细节部分：看标准，**for in window**
   4. 追本溯源
      1. 由一个点，往后深挖。类似于我们高三的做选择题，我们总能通过一些技巧得到答案，比如排除法，挨个试值。但是如果我们去深挖每一个选项背后的为什么，就会学到很多东西。在使用这种学习方法的时候，技术背景就显得尤为重要了，技术背景也是我们用来实现[学习三境界](#ment-1)之[看透背后的设计思想](#ment-1-1)
   5. **建立渐进式的知识架构**和**追本溯源**这两种学习方法，就像两个人，一个人从终点出发，一个人从起点出发，然后相遇的过程。**追本溯源**是从树叶到树根的填充，戏称"找爸爸"，**建立渐进式的知识架构**是从树根出发，一点点填充这个大树。
8. <a id="ment-1">学习三境界</a>：会用、理解标准、<a id="ment-1-1">看透背后的设计思想</a>。

