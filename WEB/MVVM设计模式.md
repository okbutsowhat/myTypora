# MVVM设计模式 (Model-View-ViewModel)

## MVVM是什么

要想了解MVVM设计模式就不得不提另外两种设计模式, MVC(Model-View-Controller)和MVP（Model-View-Presenter）

* MVC
  MVC是比较直观的架构模式，用户进行操作是，View接收用户的输入操作，传递给Controller进行业务逻辑处理，Model实现数据持久化，并将结果反馈给View，完成一次MVC模式。

  目前常见的 iOS 和 Android 开发，SDK 和与其搭配的 IDE 工具都是默认以 MVC 的方式来使用。model是数据模型，View 是视图或者说就是我们的软件界面需要去展示的东西；Controller 是用来控制Model的读取、存储，以及如何在 View上 展示数据，更新数据的逻辑控制器。

![v2-1212d374015876d55c9b3dca99ecdfba_hd](E:\1-Work and study\typora\图床\v2-1212d374015876d55c9b3dca99ecdfba_hd.jpg)