##Flutter （入门问题集合）

1. Flutter_swiper 异步请求显示 banner 时，初始 url 为空时 报错问题

   参考：https://github.com/best-flutter/flutter_swiper/issues/91

   主要：没有请求到数据的时候不要设置自动播放。`autoplay: _imageUrls.isNotEmpty`

2. 可滚动网格布局 品字型布局

   参考：`CustomScrollView` 组件

3. flutter 中的高度

   参考：

   ```dart
   /// 屏幕宽度
   	MediaQuery.of(context).size.width
   /// appbar 高度
     AppBar().preferredSize.height
   /// 系统工具栏高度 两种方式
     const kToolbarHeight -double
   	MediaQuery.of(context).padding.top
   /// 系统环境常数 是否是预发布环境
      const kReleaseMode -bool
   ```

4) flutter 组件理解

   参考：MaterialApp() 相当于 <html/> 的根组件; Scaffold() 相当于 <body/>

5) flutter 组件声明周期

   参考：

6) flutter json 解析工具 `json_model` 的使用

   主要：

7) `[]`和 `.` 运算符，与 `js` 略有不同

   参考：https://www.dartcn.com/guides/language/language-tour#其他运算符

   主要： `[]` Refers to the value at the specified index in the list。 `.` Refers to a property of an expression; example: `foo.bar` selects property `bar` from expression `foo`

8) 点击事件 `GestureDetector` 范围小

   参考：https://blog.csdn.net/cpcpcp123/article/details/94397609

   主要：behavior: HitTestBehavior.opaque,属性，可以让点击事件透过这个 Text 的区域。如果不添加这个属性，那么只能点击到文字时才会有响应。

9) 路由 带参数返回； Navigator.push 是一个异步操作

   参考：参考源码

   主要：page1 页面需要 page2 页面返回数据，page1 在跳转时需要等待 page2 返回数据

   ```dart
   /// page1 跳转到 page2
   Navigator.push(
     context,
     MaterialPageRoute(
       builder: (context) =>
       Page2(params: '来自page1'),
     ),
   ).then((data) { /// 关键：这里 data 是page2 中返回的数据
     print(data.toString());
   })；

   /// page2 返回 page1
   Navigator.pop(context, '来自page2')；
   ```

10) Widget 声明周期

    ```dart
    /// StatefullWidget vs StatelessWidget


    StatelessWidget:无中间状态变化的widget，需要更新展示内容就得通过重新new，flutter推荐尽量使用StatelessWidget
    StatefullWidget:存在中间状态变化，那么问题来了，widget不是都immutable的，状态变化存储在哪里？flutter 引入state的类用于存放中间态，通过调用state.setState()进行此节点及以下的整个子树更新
    State 生命周期


    initState(): state create之后被insert到tree时调用的
    didUpdateWidget(newWidget):祖先节点rebuild widget时调用
    deactivate():widget被remove的时候调用，一个widget从tree中remove掉，可以在dispose接口被调用前，重新instert到一个新tree中
    didChangeDependencies():
    初始化时，在initState()之后立刻调用
    当依赖的InheritedWidget rebuild,会触发此接口被调用


    build():
    After calling [initState].
    After calling [didUpdateWidget].
    After receiving a call to [setState].
    After a dependency of this [State] object changes (e.g., an[InheritedWidget] referenced by the previous [build] changes).
    After calling [deactivate] and then reinserting the [State] object into the tree at another location.


    dispose():Widget彻底销毁时调用
    reassemble(): hot reload调用
    注意事项：



    A页面push一个新的页面B,A页面的widget树中的所有state会依次调用deactivate(), didUpdateWidget(newWidget)、build()（这里怀疑是bug，A页面push一个新页面，理论上并没有将A页面进行remove操作），当然从功能上，没有看出来有什么异常
    当ListView中的item滚动出可显示区域的时候，item会被从树中remove掉，此item子树中所有的state都会被dispose，state记录的数据都会销毁，item滚动回可显示区域时，会重新创建全新的state、element、renderobject
    使用hot reload功能时，要特别注意state实例是没有重新创建的，如果该state中存在一下复杂的资源更新需要重新加载才能生效，那么需要在reassemble()添加处理，不然当你使用hot reload时候可能会出现一些意想不到的结果，例如，要将显示本地文件的内容到屏幕上，当你开发过程中，替换了文件中的内容，但是hot reload没有触发重新读取文件内容，页面显示还是原来的旧内容
    ```

11) 数据自上而下。与双向绑定的区别。

    主要：

12) Icons 图标库

    参考： https://material.io/resources/icons/?icon=account_balance&style=baseline

13) 常用组件：

    ```dart
    1. 加载： CircularProgressIndicator
    2. 下拉刷新：RefreshIndicator
    3. 事件：GestureDetector
    4. 布局：CustomScrollView
    ```

14. 吸顶滚动的实现方案

    主要：`1.SliverPersistentHeade` 可配合 `NestedScrollView` 使用

    参考：`CustomScrollView` 的属性 `shrinkWrap: true,` 会改变 `SliverPersistentHeader` 浮动的层级。

15. 在 List.map() 中取到 index 的方法

    参考：https://stackoverflow.com/questions/54898767/enumerate-or-map-through-a-list-with-index-and-value-in-dart

    主要：有一种`asMap`方法可以将列表转换为映射，其中键是索引，值是索引中的元素。请在[这里](https://api.dartlang.org/stable/2.2.0/dart-core/List/asMap.html)查看文档。例：

    ```dart
    List _sample = ['a','b','c'];
    _sample.asMap().forEach((index, value) => f);
    ```

    ```dart
    List<Widget> _buildWidgets(List<Object> list) {
        return list
            .asMap()
            .map((index, value) =>
                MapEntry(index, _buildWidget(index, value)))
            .values
            .toList();
    }
    ```

    或者，您可以创建一个同步生成器函数以返回一个可迭代

    ```dart
    Iterable<MapEntry<int, T>> enumerate<T>(Iterable<T> items) sync* {
      int index = 0;
      for (T item in items) {
        yield MapEntry(index, item);
        index = index + 1;
      }
    }

    //and use it like this.
    var list = enumerate([0,1,3]).map((entry) => Text("index: ${entry.key}, value: ${entry.value}"));
    ```

16. list 二维展开

    参考：https://juejin.im/post/5dac7b93e51d45248c7b5182#heading-16

    主要：`List.expand()`

17. 网格布局 子元素宽高适配？？？

18. tabView 缓存问题？？？

19. 通常在用到 `PageView` + `BottomNavigationBar` 或者 `TabBarView` + `TabBar` 的时候大家会发现当切换到另一页面的时候, 前一个页面就会被销毁, 再返回前一页时, 页面会被重建, 随之数据会重新加载, 控件会重新渲染

    参考：https://juejin.im/post/5b73c3b3f265da27d701473a

20. 键盘弹起导致布局 overflow

    ```dart
    flutter中关于软键盘弹起导致的问题

    当布局高度写死时，例如设置为屏幕高度，这时候键盘弹起页面上会出现布局overflow的提示
    软键盘弹起后遮挡输入框
    原因：在flutter中，键盘弹起时系统会缩小Scaffold的高度并重建

    解决问题1中overflow提示的两种办法：
    1）把Scaffold的resizeToAvoidBottomInset属性设置为false，这样在键盘弹出时将不会resize

    2）把写死的高度改为 原高度 - MediaQuery.of(context).viewInsets.bottom，键盘弹出时布局将重建，而这个MediaQuery.of(context).viewInsets.bottom变量在键盘弹出前是0，键盘弹起后的就是键盘的高度

    解决问题2的办法：
    将输入框放进可滚动的Widget中即可，当输入框获取焦点后，系统会自动将它滑动到可视区域
    ```

21. TextField 焦点与 hint 不对齐问题

    ```dart
    style: TextStyle(
      fontSize: 16,
      textBaseline: TextBaseline.alphabetic,
    ),
    ```

22. ScreenUtil 打 release 包的问题，大概因为 ScreenUtil 初始化在渲染首页之后，导致界面文字或字体显示不全 (不推荐使用 ScreenUtil)

    参考：https://github.com/OpenFlutter/flutter_screenutil/issues/39；https://github.com/OpenFlutter/flutter_screenutil/issues/63#issuecomment-552696609。

    主要：两种解决方案

    ```dart
    之前有人在官方issue里提过，官方更推荐使用MediaQuery, window获取数据不依赖Widget的生命周期，这就会产生一些问题，例如使用debug版本因为程序运行缓慢而拿到宽高数据，而release版本应用速度提升后就无法获取。我现在是在iniState中使用SchedulerBinding.instance.addPostFrameCallback()的回调进行初始化，希望对您有帮助。
    ```

    ```dart
    @hzysunrain 我试过在didChangeMetrics中初始化ScreenUtil , 但是它的触发时间晚于build (在我尝试的几次)
    哪怕我在Myapp中的didChangeMetrics都要在home的build方法之后才调用

    比较理想的就是在home(一般作为启动页面)中初始化 , 但是不使用ScreenUtil(避免特殊情况拿不到屏幕数据), 在启动页的下一个页面应该就可以直接使用了
    ```

23. flutter 三种编译模式：

    参考：https://github.com/cyndibaby905/34_multi_env

    Flutter 支持 3 种运行模式，包括 Debug、Release 和 Profile。在编译时，这三种模式是完全独立的。首先，我们先来看看这 3 种模式的具体含义吧。


    * Debug 模式对应 Dart 的 JIT 模式，可以在真机和模拟器上同时运行。该模式会打开所有的断言（assert），以及所有的调试信息、服务扩展和调试辅助（比如 Observatory）。此外，该模式为快速开发和运行做了优化，支持亚秒级有状态的 Hot reload（热重载），但并没有优化代码执行速度、二进制包大小和部署。flutter run --debug 命令，就是以这种模式运行的。



    * Release 模式对应 Dart 的 AOT 模式，只能在真机上运行，不能在模拟器上运行，其编译目标为最终的线上发布，给最终的用户使用。该模式会关闭所有的断言，以及尽可能多的调试信息、服务扩展和调试辅助。此外，该模式优化了应用快速启动、代码快速执行，以及二级制包大小，因此编译时间较长。flutter run --release 命令，就是以这种模式运行的。



    * Profile 模式，基本与 Release 模式一致，只是多了对 Profile 模式的服务扩展的支持，包括支持跟踪，以及一些为了最低限度支持所需要的依赖（比如，可以连接 Observatory 到进程）。该模式用于分析真实设备实际运行性能。flutter run --profile 命令，就是以这种模式运行的。由于 Profile 与 Release 在编译过程上几乎无差异。

    ```datt
    // 如何通过断言识别应用的编译模式。
    // 通过 Debug 与 Release 模式的介绍，我们可以得出，Release 与 Debug 模式的一个重要区别就
    // 是，Release 模式关闭了所有的断言。因此，我们可以借助于断言，写出只在 Debug 模式下生效的代
    // 码。如下所示，我们在断言里传入了一个始终返回 true 的匿名函数执行结果，这个匿名函数的函数体
    // 只会在 Debug 模式下生效：

      assert(() {
        //Do sth for debug
        return true;
      }());

    // 如何通过编译常数识别应用的编译模式。
    // 如果说通过断言只能写出在 Debug 模式下运行的代码，而通过 Dart 提供的编译常数，我们还可以写
    // 出只在 Release 模式下生效的代码。Dart 提供了一个布尔型的常量 kReleaseMode，用于反向指
    // 示当前 App 的编译模式。

      if(kReleaseMode){
        //Do sth for release
      } else {
        //Do sth for debug
      }
    ```

24. 关于 flutter 开发 app

    - APP 开发前关于请求工具类，主题，路由管理，屏幕适配和国际化这类框架上的要提前做好封装。
    - debug 和 release 打包后有区别，在开发中不忘测试 release，特别是一些需要在哪里考虑初始化的工具类。由于 debug 和 release 在运行上速度上的差异，有些初始化顺序导致的访问不到方法等问题，在 debug 上可能没有表现，但是 release 上就会有问题了。
    - 关于代码编写，在 build()中要尽量保证组件代码结构看起来扁平化整洁。将较深层的 Widget 用变量提取替代。不腿脚以方法参数形式层层嵌套。
    - 用 json_model 做好 json 转 dart 类。尽量都要使用确定类型声明变量。

25. 如何触发一帧的绘制回调

    主要：

    ```dart
    @override
    void initState() {
      super.initState();
      widgetsBinding=WidgetsBinding.instance;
      widgetsBinding.addPostFrameCallback((callback){
        widgetsBinding.addPersistentFrameCallback((callback){
          print("addPersistentFrameCallback be invoke");
          //触发一帧的绘制
          widgetsBinding.scheduleFrame();
        });
      });
    }
    ```


    参考：https://blog.csdn.net/baoolong/article/details/85097318

26. flutter 生命周期（交互）


    主要：

    - 组件渲染：组件的构建渲染=> [传送门](https://juejin.im/post/5c8729756fb9a049e0642d91)

    - 前后台交互:

    ```dart
    abstract class WidgetsBindingObserver {

       //路由弹出Future
      Future<bool> didPopRoute() => Future<bool>.value(false);

        //新的路由Future
      Future<bool> didPushRoute(String route) => Future<bool>.value(false);

        //系统窗口相关改变回调，例如旋转
      void didChangeMetrics() { }

        //文字系数变化
      void didChangeTextScaleFactor() { }

        //本地化语言变化
      void didChangeLocales(List<Locale> locale) { }

        //生命周期变化
      void didChangeAppLifecycleState(AppLifecycleState state) { }

        //低内存回调
      void didHaveMemoryPressure() { }

        //当前系统改变了一些访问性活动的回调
      void didChangeAccessibilityFeatures() {}
    }

    ```



    - 其他回调：1.单次Frame绘制回调(addPostFrameCallback)；2.  实时Frame绘制回调(addPersistentFrameCallback)

    参考：https://juejin.im/post/5c872a366fb9a049cd5508fa

27. 数组拼接

    ```dart
    list = List.from(list)..addAll(newList);
    ```

28. 是否触底

    ```DART
    void listener() {
        ScrollPosition position = _scrollController.position;
        if (position.pixels == position.maxScrollExtent) {
          print('触底啦');
          getMore();
        }
      }
    ```

29. 工具类初始化问题：

    参考：

    主要：有些需要异步初始化的工具类，在调用前需要先初始化，建议在这些类初始化完成后再添加需要加载的页面组件。保证页面组件在 init 前，相关的工具类已经初始化完成。

30. 修改`button` 的大小

    主要：`ButtonTheme`

    ```dart
    ButtonTheme(
      minWidth: 200.0,
      height: 100.0,
      child: RaisedButton(
        onPressed: () {},
        child: Text("test"),
      ),
    );
    ```

31. 限制 flutter 容器的最大宽度

    参考：https://stackoverflow.com/questions/55185288/limit-max-width-of-container-in-flutter

    主要：

    ```dart
    Widget build(context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        Container(
          constraints: BoxConstraints(minWidth: 100, maxWidth: 200),
          padding: EdgeInsets.all(10),
          decoration: BoxDecoration(
            color: color ?? Colors.blue,
            borderRadius: BorderRadius.circular(10)
          ),
          child: msg
        )
      ],
    );
    }
    ```

32. [error]: /_ Vertical viewport was given unbounded height_/

    _（使用插件 flutter_staggered_grid_view 实现瀑布流时，需要在 CustomScrollView 下插入一段瀑布流布局，遇到此问题，且在 flutter_staggered_grid_view 组件中无法获取到当前页面的 scollControl 可在组件内实例化一个滚动控制器才可滚动）_

    参考：https://stackoverflow.com/questions/50252569/vertical-viewport-was-given-unbounded-height

    主要：使用 ListView 系列组件时报错：垂直视口的高度不受限制

    ```dart
    // adding this two lines or set parent height
    ListView.builder(
        scrollDirection: Axis.vertical,
        shrinkWrap: true,
    ...
    ```

33. How to Move bottomsheet along with keyboard which has textfield(autofocused is true)?

    参考：https://stackoverflow.com/questions/53869078/how-to-move-bottomsheet-along-with-keyboard-which-has-textfieldautofocused-is-t

    主要：

    ```dart
    showModalBottomSheet(context: context, builder: (context) {
      return Container(
        child: Padding(
          padding: EdgeInsets.only(
    			bottom: MediaQuery.of(context).viewInsets.bottom),
            child: TextField(autofocus: true,),),);
       });
    ```

34. flutter 组件声明周期调用次数

| 阶段                  | 调用次数 | 是否支持 setState                |
| --------------------- | -------- | -------------------------------- |
| 构造函数              | 1        | 否                               |
| initState             | 1        | 无效(使用 setState 和不使用一样) |
| didChangeDependencies | >=1      | 无效                             |
| didUpdateWidget       | >=1      | 无效                             |
| deactivate            | >=1      | 否                               |
| dispose               | 1        | 否                               |

35. 问题 3：继承、接口与混入的相关问题。


    **第一**，你是怎样理解父类继承、接口实现和混入的？我们应该在什么场景下使用它们？



    父类继承、接口实现和混入都是实现代码复用的手段，我们在代码中应该根据不同的需求去使用。其中：



    - 在父类继承中，子类复用了父类的实现，适用于两个类的整体存在逻辑层次关系的场景；

    -

    - 在接口实现中，类复用了接口的参数、返回值和方法名，但不复用其方法实现，适用于接口和类在行为存在逻辑层次关系的场景；

    -

    - 而混入则可以使一个类复用多个类的实现，这些类之间无需存在父子关系，适用于多个类的局部存在逻辑层次关系的场景。



    **第二** ，在父类继承的场景中，父类子类之间的构造函数执行顺序是怎样的？如果父类有多个构造函数，子类也有多个构造函数，如何从代码层面确保父类子类之间构造函数的正确调用？



    默认情况下，子类的构造函数会自动调用父类的默认构造函数，如果需要显式地调用父类的构造函数，则需要在子类构造函数的函数体开头位置调用。但，如果子类提供了初始化参数列表，则初始化参数列表会在父类构造函数之前执行。



    构造函数之间，有时候会有一些相同的逻辑。如果把这些逻辑分别写在各个构造函数中，会有些累赘，所以构造函数之间是可以传递的，相当于填充了某个构造函数的参数，从而实现类的初始化。因此可以传递的构造函数是没有方法体的，它们只会在初始化列表中，去调用另一个构造函数。



    如果子类与父类存在多个构造函数，通常是为了简化类的初始化代码，将部分不需要的属性设置为默认值。因此，我们只要能确保每条构造函数的初始化路径都不会有属性被遗漏即可。**一个好的做法是**，依照构造函数的参数个数，将参数少的构造函数转发至参数多的构造函数中，由参数最多的构造函数统一调用父类参数最多的那个构造函数。

36. 问题 8：在 ListView 中，如何提前缓存子元素？

    `ListView` 构造函数中有一个 `cacheExtent` 参数，即预渲染区域长度。`ListView` 会在其可视区域的两边留一个 `cacheExtent` 长度的区域作为预渲染区域，相当于提前缓存些元素，这样当滑动时就可以迅速呈现了。

37. 问题 9：Row 与 Column 自身的大小是如何决定的？当它们嵌套时，又会出现怎样的情况呢？

    Row 与 Column 自身的大小由父 Widget 的大小、子 Widget 的大小，以及 mainSize **共同决定**。

    Row 和 Column 只会在主轴方向占用尽可能大的空间（max：屏幕方向主轴大小或父 Widget 主轴方向大小；min：所有子 Widget 组合在一起的主轴方向大小），而纵轴的长度则取决于它们最大子元素的长度。

    如果 Row 里面嵌套 Row，或者 Column 里面嵌套 Column，只有最外层的 Row 或 Colum 才会占用尽可能大的空间，里层 Row 或 Column 占用的空间为**实际大小**。

38. 问题 14：请分别概括属性传值、InheritedWidget、Notification 与 EventBus 的特点。

    **属性传值**适合在同一个视图树中使用，传递方向由父及子，通过构造方法将值以属性的方式传递过去，简单高效。

    其缺点是，涉及跨层传递时，属性可能需要跨越很多层才能传递给子组件，导致中间很多并不需要这个属性的组件，也得接收其子 Widget 的数据，繁琐且冗余。


    **InheritedWidget** 适用于子 Widget 主动向上层拿数据的场景，传递方向由父及子，可以实现跨层的数据读共享。InheritedWidget 也可以实现写共享，需要在上层封装写数据的方法供下层调用。

    其优点是，数据传输方便，无代码侵入即可达到逻辑和视图解耦的效果；

    而其缺点是，如果层次较深，刷新范围过大会影响性能。



    **Notification** 适用于子 Widget 向父 Widget 推送数据的场景，传递方向由子及父，可以实现跨层的数据变更共享。

    其优点是，多个子元素的同一事件可由父元素统一处理，多对一简单；

    而其缺点是，Notification 的自定义过程略繁琐。



    **EventBus** 适用于无需存在父子关系的实体之间通信，订阅者需要显式地订阅和取消。

    其优点是，能够支持任意对象的传递，一对多的方式实现简单；

    而其缺点是，订阅管理略显繁琐。

39. 问题 6：**State 构造函数和 initState 的差异是什么？**

    State 构造函数调用时，Widget 还未完成初始化，因此仅适用于一些与 UI 无关的数据初始化，比如父类传入的参数加工。

    而 initState 函数调用时，StatefulWidget 已经完成了 Widget 树的插入工作，因此与 Widget 相关的一些初始化工作，比如设置滚动监听器则必须放在 initState。

40. 问题 5：**Widget、Element 和 RenderObject 之间是什么关系？**你能否在 Android/iOS/Web 中找到对应的概念呢？

    Widget 是数据配置，RenderObject 负责渲染，而 Element 是一个介于它们之间的中间类，用于渲染资源复用。

    Widget 和 Element 是一一对应的，但 RenderObject 不是，只有实际需要布局和绘制的控件才会有 RenderObject。


    这三个概念在 iOS、Android 和 Web 开发中，对应的概念分别是：



    - **在 iOS 中**，Xib 相当于 Widget，UIView 相当于 Element，CALayer 相当于 renderObject；

    - **在 Android 中**，XML 相当于 Widget，View 相当于 Element，Canvas 相当于 renderObject；

    - **在 Web 中**，以 Vue 为例，Vue 的模板相当于 Widget，virtual DOM 相当于 Element，DOM 相当于 RenderObject。



41. 问题 1：直接在 build 函数里以内联的方式实现 Scaffold 页面元素的构建，好处是什么？

    这样做的最大好处是，各个组件之间可以直接共享页面的状态和方法，页面和组件间不再需要把状态数据传来传去、多级回调了。


    不过这种方式也有缺点，一旦数据发生变更，Flutter 会重建整个大 Widget（而不是变化的那部分），所以会对性能产生些影响。

42. flutter 像素 devicePixelRatio 属性

    [double](https://api.flutter.dev/flutter/dart-core/double-class.html) devicePixelRatio

    每个逻辑像素的设备像素数。此数字可能不是 2 的幂。实际上，它甚至可能不是整数。例如，Nexus 6 的设备像素比为 3.5。

    设备像素也称为物理像素。逻辑像素也称为与设备无关或与分辨率无关的像素。

    根据定义，物理显示器的每厘米大约有 38 个逻辑像素，或者每英寸大约有 96 个逻辑像素。[devicePixelRatio](https://api.flutter.dev/flutter/dart-ui/Window/devicePixelRatio.html)返回的值最终是从硬件本身，设备驱动程序或存储在操作系统或固件中的硬编码值获得的，有时可能不准确，甚至有很大的余量。

    Flutter 框架在逻辑像素中运行，因此很少需要直接处理此属性。

    当此更改时，将调用[onMetricsChanged](https://api.flutter.dev/flutter/dart-ui/Window/onMetricsChanged.html)。

    也可以看看：

    - [WidgetsBindingObserver](https://api.flutter.dev/flutter/widgets/WidgetsBindingObserver-class.html)，用于在小部件层观察此值更改的机

    ```dart
    double get devicePixelRatio => _devicePixelRatio;
    ```

43) Duration

    ```dart
     /**
       * Returns a string representation of this `Duration`.
       *
       * Returns a string with hours, minutes, seconds, and microseconds, in the
       * following format: `HH:MM:SS.mmmmmm`. For example,
       *
       *     var d = new Duration(days:1, hours:1, minutes:33, microseconds: 500);
       *     d.toString();  // "25:33:00.000500"
       */
    ```
