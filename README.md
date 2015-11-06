# Swift设计模式 (iOS)

我们将会通过完成一个完整的应用，展示音乐专辑和专辑的相关信息来学习设计模式在 Swift 中的实现。

通过这个应用，我们会接触一些 Cocoa 中常见的设计模式：

- 创建型 (Creational)：单例模式 (Singleton)
- 结构型 (Structural)：MVC、装饰者模式 (Decorator)、适配器模式 (Adapter)、外观模式 (Facade)
- 行为型 (Behavioral)：观察者模式 (Observer)、备忘录模式 (Memento)

## 整理排版说明

在 `Xcode 7` 中进行编码测试，升级为 `Swift 2.0` 解决原文中出现的问题，保证了语句与Demo的可用性

在线阅读: http://swift-design-patterns.books.yourtion.com/

下载电子书: https://www.gitbook.com/book/yourtion/swiftdesignpatterns/details

直接下载：[PDF](https://www.gitbook.com/download/pdf/book/yourtion/swiftdesignpatterns)、[EPub](https://www.gitbook.com/download/epub/book/yourtion/swiftdesignpatterns)、[Mobi](https://www.gitbook.com/download/mobi/book/yourtion/swiftdesignpatterns)

**有修改建议优化请[提交Issus](https://github.com/yourtion/SwiftDesignPatterns/issues/new)，或请直接Fork：<https://github.com/yourtion/SwiftDesignPatterns/> 进行修改并申请 Pull Request。**

项目Demo：https://github.com/yourtion/SwiftDesignPatterns-Demo1

## 更新声明

本书整理排版自：

- [iOS 中的设计模式 (Swift版本) Part 1](http://blog.callmewhy.com/2014/12/29/introducing-ios-design-patterns-in-swift-part-1/)
- [iOS 中的设计模式 (Swift版本) Part 2](http://blog.callmewhy.com/2015/03/01/introducing-ios-design-patterns-in-swift-part-2/)

原文翻译自 [Introducing iOS Design Patterns in Swift – Part 1/2](http://www.raywenderlich.com/86477/introducing-ios-design-patterns-in-swift-part-1) 和 [Introducing iOS Design Patterns in Swift – Part 2/2](http://www.raywenderlich.com/90773/introducing-ios-design-patterns-in-swift-part-2) ，本教程 [objc](http://www.raywenderlich.com/46988/ios-design-patterns) 版本的作者是 Eli Ganem ，由 Vincent Ngo 更新为 Swift 版本。

Update 04/22/2015: Updated for Xcode 6.3 and Swift 1.2.

Update note: This tutorial was updated for iOS 8 and Swift by Vincent Ngo. Original post by Tutorial team member Eli Ganem.

## GitBook 排版

**Yourtion**
- yourtion@gmail.com
- https://github.com/yourtion

