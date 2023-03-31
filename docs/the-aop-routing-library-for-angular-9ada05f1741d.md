# 面向角度的 AOP 路由库

> 原文：<https://medium.com/analytics-vidhya/the-aop-routing-library-for-angular-9ada05f1741d?source=collection_archive---------10----------------------->

![](img/dd404273cb7ab12a6295d02a91d57b7d.png)

有一个令人惊叹的新 npm 库，名为 [**aop-routing**](https://www.npmjs.com/package/aop-routing) ，它增强并为 Angular 应用程序中的导航和路由带来了许多简洁的功能。

# aop 路由到底是什么？

**直接取自文档:** Aop-Routing 通过**typescript decorator 的易用性，提供了在 Angular 中执行[命令式和 Popstate 导航](/analytics-vidhya/angular-routing-imperative-vs-popstate-7d254b495c54)操作的能力，而不需要注入或导入 Angular Router 对象。**

总而言之，aop-routing 库允许您在组件之间执行导航，而不必将 Router 对象导入或注入到组件中，并且还提供了其他简洁的特性，比如在运行时动态更新路由表。执行导航就像在方法顶部放置一个装饰器一样简单，就是这样！

> Aop-Routing 提供了在 Angular 中执行[命令性和 Popstate 导航](/analytics-vidhya/angular-routing-imperative-vs-popstate-7d254b495c54)操作的能力，这是通过简单的 typescript decorators 实现的，不需要导入 Angular Router 对象。

## 以下是 aop 路由提供的功能列表:

*   使用装饰器的命令式导航
*   使用装饰器的 PopState 导航
*   覆盖默认导航逻辑的自定义导航逻辑
*   运行时向路由表动态添加新路径
*   在运行时动态更改路径的组成部分
*   运行时动态添加/删除激活防护

# 让我们看看如何将这个库安装并集成到我们的应用程序中

**注意**:AOP 库要求安装**angular 8.1**或更高版本！

1.  将 aop 路由库安装到您的 angular 应用程序中

```
npm install aop-routing
```

2.安装完库后，将 **AopRoutingModule** 添加到应用程序的顶层/根模块导入数组中。

```
imports: [
   ...
    AopRoutingModule
  ]
```

3.将 **AopNavigationService** 依赖关系添加到顶级/根模块构造函数中。

```
export class AppModule {
  constructor(private navigationService: AopNavigationService) {}
 }
```

这就是将 aop-routing 库集成到 angular 应用程序所需的全部内容。

现在让我们看看如何使用 aop-routing 库和它很酷的特性！

## 我将使用下面的路由表来演示这些功能

```
const routes: Routes = [
**{path: 'page1', component: Page1Component, canActivate: [TestGuard,]},
{path: 'page2', component: Page2Component },
{path: 'page3', component: Page3Component }**
];
```

**转到下一页:**

使用 aop-routing 库，当您需要路由到下一个页面或组件时，只需在您想要执行导航的函数上使用 **RouteNext()** decorator 即可。

**下面的例子将在 testMethod1 执行结束时路由到第 2 页——注意没有注入或使用路由器对象**

```
import { Component} from '@angular/core';@Component({
...
})export class Page1Component {constructor() {}**@RouteNext('page2')**
public testMethod1() {
...some logic...
 }
}
```

如果您的导航是基于动态数据的，这也可以通过让您的方法返回一个'**字符串**或' **AopNavigator** 对象来实现。装饰器将使用返回值来执行路由。

```
//Routing dynamically with RouteNext Decorator by returning a stringimport { Component} from '@angular/core';@Component({
...
})export class Page1Component {
constructor() {}**@RouteNext()**
public testMethod1(): string {
...some logic...
**return 'page2';** }
}-----------------------------------------------------------------
// Routing dynamically with RouteNext Decorator by returning an 
// AopNavigatorimport { Component} from '@angular/core';@Component({
...
})export class Page1Component {
constructor() {}**@RouteNext()**
public testMethod1(): string {
  ...some logic...
  **const obj: AopNavigator = {
     destinationPage: 'Test2',
   };
  return obj;** }
}
```

aop-routing 也有后缀为 **Async** 的 decorators(例如 **RouteNextAsync** )，可以与[异步](/analytics-vidhya/asynchronous-programming-in-a-nutshell-theory-d5fd07cf3b22)方法一起使用。

```
// The RouteNextAsync decorator will route to page2 by subscribing // to testMethod1 and using it's string value to perform the routing
**@RouteNextAsync()**
public testMethod1(): **Observable<string>** {
  ...some logic...
 **return of(1, 2, 3).pipe(
   switchMap(x => {
     return of('page2');
   })
 );**
}----------------------------------------------------------------
// The RouteNextAsync decorator will route to page2 by subscribing // to testMethod1 and using the returned AopNavigator object value // to perform the routing**@RouteNextAsync()**
public testMethod1(): **Observable<AopNavigator>** {
  ...some logic...

   **const obj: AopNavigator = {
    destinationPage: 'Test2',
  };**

 **return of(1, 2, 3).pipe(
   switchMap(x => {
     return of(obj);
   })
 );**
}
```

**RouteBack** 和**route back sync**decorator，可用于执行 [**popstate**](/analytics-vidhya/angular-routing-imperative-vs-popstate-7d254b495c54) 导航到上一页。

```
//testMethod1 will navigate back to previous page after execution
**@RouteBack()**
public testMethod1() {
 ...some logic...
}
------------------------------------------------------------------- //Will navigate to the previous page after the asynchronous //execution of testMethod1
**@RouteBackAsync()**
public testMethod1() {
 return of(...some async operations).pipe(
 ...rxjs operators...)
}
```

**route state**和**route state async** AOP-routing 库还提供了使用 popstate 导航来路由到浏览器历史中特定状态的能力。

```
// Will traverse 2 states **backwards** of the browser history state 
// equivalent to hitting the browser back button twice **@RouteToState(-2)**
public testMethod1() {
 ...some logic...
}------------------------------------------------------------------//Will traverse 2 states forward of the browser history state **@RouteToState(2)**
public testMethod1() {
 ...some logic...
}------------------------------------------------------------------// Will subscribe to the targeted method and use the returned value to traverse 2 states backwards of the browser history state after end of targetted method.**@RouteToStateAsync()**
public testMethod1(): **Observable<number>** {
  ...some logic...
  return of(1, 2, 3).pipe(
   switchMap(x => {
     return of(-2);
   })
 );
}
------------------------------------------------------------------//Will make the decorator subscribe to the AopNavigator object returned from the targetted method and use the destinationPage property value to perform popstate navigation traversal of the browser history state. **@RouteToStateAsync()**
public testMethod1(): **Observable<AopNavigator>** {
  ...some logic...

   **const obj: AopNavigator = {
    destinationPage: -2',
  };**

  **return of(1, 2, 3).pipe(
   switchMap(x => {
     return of(obj);
   })
 );**
}
```

**AopNavigator** 接口具有其他可选属性，可用于增强 aop-routing 导航。

*   **destinationPage:** 可以向该属性传递一个字符串或数值，该字符串或数值可用于 RouteNext、RouteNextAsync、RouteToState 和 RouteToStateAsync 装饰器来执行导航。
*   **navigationExtra:** 该属性接受一个 [Angular NavigationExtras 对象](https://angular.io/api/router/NavigationExtras)，以允许额外的选项来修改 RouteNext 和 RouteNextAsync 装饰器的路由器导航策略。
*   **预处理:**该属性采用引用函数。该函数将在装饰者执行任何导航之前执行。
*   **param :** 该属性将接受任何类型的值，该值可用作预处理属性中传递的函数的参数。

![](img/05c60864da675a9df9db443bd5af2646.png)

## 自定义逻辑

如果你想更好地控制导航，这也是可以做到的。**AOP-routing 库为用户提供了提供他们自己的定制实现来覆盖默认导航逻辑的能力**。

这可以通过简单的 3 个步骤来完成。

1.  创建一个扩展抽象类**aopbaseenavigation**的类。

```
export class SampleClass extends **AopBaseNavigation** {}
```

2.实现 AopBaseNavigation 抽象类所需的抽象方法。

*   goToNextPage()
*   goToPreviousPage()
*   goToState()

```
export class SampleClass extends **AopBaseNavigation** {
 public **goToNextPage**(...) {
  ...custom logic...
}

 public **goToPreviousPage**(...) {
  ...custom logic...
}

 public **goToState**(...) {
  ...custom logic...
}
```

3.在顶层/根模块中，将 **AopProxyNavigationService 添加到 providers 数组中，并将 useClass 设置为新创建的类**

```
@NgModule({
  imports: [
    ...
    AopRoutingModule
  ],
  **providers**: [**{provide: AopProxyNavigationService, useClass: SampleClass}**],
})
```

现在，SampleClass 将覆盖默认的导航逻辑。所以装饰者将调用 SampleClass 的方法，而不是默认的逻辑。

## 动态变化

aop-routing 库最酷的特性之一是能够在应用程序运行时修改路由表。

**注**:文档页面上注明以下功能仍处于实验阶段。

要打开实验特性，您需要将一个将 **experimentalNav** 属性设置为 true 的对象传递给根方法的**AopRoutingModule**到顶层/根模块:

```
@NgModule({
  ...
  imports: [
    ...
    AopRoutingModule.**forRoot({expirementNav: true})**
  ],
  ...
})
```

**向路由表添加新路径:**

场景:假设在应用程序运行时，对于特定的流，我们想要添加新的 route path 对象，以便应用程序导航到该对象。我们将使用 aop-routing 库在应用程序执行期间向上面创建的路由表添加一个新路径。

路径将是**第 4 页**并且它应该路由到**第 4 页组件:**

1.  创建一个 **RouteTransform** 对象并设置 path 和* *component* 属性:

```
const routeTransform: RouteTransform = {
    path: 'page4',
    component: Page4Component
 };
```

2.在目标函数的 RouteNext 或 RouteNextAsync 声明器中，返回一个设置了 **routeTransform** 属性的 **AopNav** 对象。

```
// aop-routing library will use this object and add this new path to
// the routing table at run time and navigate to it.**@RouteNext()**
public testMethod() {
  **const routeTransform: RouteTransform = {
    path: 'page4',
    component: Page4Component
 };
  return {routeTransform}**
}
```

**在运行时改变路径的组件** 使用 aop-routing，我们还可以在运行时改变现有路径的组件。回想一下上一节中我们的路由表，**页面 1** 将路由到**页面 1 组件。**

假设我们想在运行时更改组件，转到 **Page4Component** 。

```
// aop-routing will override the default component(Page1Componet)  // set for page1 path and instead set attach Page4Component to page1
@RouteNext()
public testMethod() {
  const routeTransform: RouteTransform = {
   ** path: 'page1',
    component: Page4Component**
 };
  return {routeTransform}
}
```

阅读这篇文章以获得关于添加和移除激活防护的更详细的解释: [**在角度**](/javascript-in-plain-english/dynamically-add-and-remove-canactivate-route-guards-in-angular-e7820ab4e061) 中动态添加和移除激活防护

**在运行时添加激活防护:** 我们也可以在运行时将激活防护添加到路由路径中

下面的例子将把 **guard1** 和 **guard2** 动态添加到 page2 route path 并路由到它。

```
**@RouteNext()**
public testMethod() {
  **const routeTransform: RouteTransform = {
    path: 'page2',
    canActivateGuards: [guard1, guard2]
 };
  return {routeTransform}**
}
```

**在运行时移除激活防护:** 激活防护也可以在运行时从路径中移除。它与上面的代码相同。aop-routing 库能够检测并删除路由表中是否存在提供的保护。

**运行时禁用所有可激活防护:**
删除与路径相关的所有可激活防护与添加防护的步骤相同。相反，应将 canActivateGuards 属性设置为空数组。

```
**@RouteNext()**
public testMethod() {
  const routeTransform: RouteTransform = {
    path: 'page1',
    **canActivateGuards: []**
 };
  return {routeTransform}} 
```

**注意**:对路由表的更改不会持久化。在导航之后，路由表恢复到其原始状态。

aop-routing library 是一个伟大的工具，它极大地增强了 angular 开发人员的导航能力，并使导航变得更加容易。

你用过 aop 路由库吗？在下面评论，让我知道你的想法！

![](img/9e17bda49757f233a55bd03d712f39bc.png)

如果你喜欢这篇文章，一定要点击几下拍手按钮，也看看我下面的其他文章！

*   [**地图 vs WeakMap**](/p/b324d20cc277)
*   [**动态添加和移除角度**](/javascript-in-plain-english/dynamically-add-and-remove-canactivate-route-guards-in-angular-e7820ab4e061) 中的可激活防护
*   [**简单地说异步编程(理论)**](https://ericsarpong.medium.com/asynchronous-programming-in-a-nutshell-theory-d5fd07cf3b22)
*   [**什么是类型脚本元组**](/@ericsarpong/what-is-a-typescript-tuple-814a016f61fd)
*   [**深度潜入 Javascript 图**](/@ericsarpong/deep-dive-into-javascript-map-object-24c012e0b3fe)