# 如何使用 React-query 发布和获取数据

> 原文：<https://medium.com/analytics-vidhya/how-to-post-and-fetch-data-using-react-query-4c3280c0ef96?source=collection_archive---------0----------------------->

![](img/0982cb8c28c34d9f3dbecfac07504d0d.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

在本文中，您将了解如何设置 react-query，以便在用 typescript/javascript 编写的应用程序和 react 功能组件中获取和发送数据。此外，您将了解什么是 react-query 及其优势。

## **什么是 react-query？**

React-query 是一个很棒的库，它解决了应用程序中管理服务器状态和缓存的问题，根据官方文档“React Query 经常被描述为 React 缺少的数据获取库，但用更专业的术语来说，它使 React 应用程序中的**获取、缓存、同步和更新服务器状态**变得轻而易举。”更多信息请参见 [**反应-查询**](https://react-query.tanstack.com/overview) **。**

## **使用 react-query 的优势**

通过数据缓存提高应用程序性能。此外，只要有更新(后台更新)，就允许在后台提取数据，当窗口重新聚焦时重新提取数据，并且还提供了默认设置，在出现错误的情况下重试失败的查询请求三(3)次。

首先，让我们用 npm 或 yarn 安装 react-query **版本 3.5.12**

```
npm i react-query*or*yarn add react-query
```

安装将用于样式、列表和表单验证的材料 ui、mui-datatables 和 react-hook-form，有关更多信息，请访问相应的文档。

```
npm install @material-ui/core
npm install mui-datatables --save
npm install react-hook-form
```

用下面的代码片段安装 React Query Devtools，这是可选的。

```
yarn add react-query-devtools
```

## **配置客户端提供商**

现在让我们通过包装 QueryClientProvider 来配置 app.tsx 组件，因为它需要位于负责获取数据的组件之上。QueryClientProvider 应采用 QueryClient 的实例，QueryClient 可用于配置 QueryCache 和 MutationCache 实例并与之交互。

## **用 axios 发布数据**

让我们开始创建负责发布数据的组件 employeeForm.tsx，我们将使用一个名为 beeceptor 的虚拟 rest api 来处理 axios 的 post 请求。了解[**bee ceptor**](https://beeceptor.com/)**。**

创建一个包含 post 请求的函数，如下面的代码片段所示。

```
const createEmployee = async (data: Employee) => {const { data: response } = await axios.post('https://employee.free.beeceptor.com/create', data);return response.data;};
```

现在创建一个任意数量字段的雇员接口。

```
interface Employee{
    name:string;
    job:string;
    id:string;
}
```

在我们的功能组件中，我们将从 react-query 导入一个名为 **useQueryClient** 的钩子，它返回一个 **QueryClient** 实例。useQueryClient 挂钩将用于使查询无效，在这种情况下，查询被认为是陈旧的，如果当前正在通过相关挂钩呈现，也可以在后台重新提取。

```
import {useQueryClient} from 'react-query'
const queryClient = useQueryClient()
```

接下来，我们导入 useMutation 挂钩，用于应用程序中任何 CRUD 操作。我们将从 useMutation 钩子中析构 mutate 和 isLoading，钩子将带一个负责发布的函数参数和几个选项。将使用的选项有 onSuccess、onError 和 onSettled。

每次变异成功都会触发 onSuccess，变异过程中出现错误会触发 onError，变异成功与否都会触发 onSettled。

```
const { mutate, isLoading } = useMutation(createEmployee, {
   onSuccess: data => {
      console.log(data);
      const message = "success"
      alert(message)
},
  onError: () => {
       alert("there was an error")
},
  onSettled: () => {
     queryClient.invalidateQueries('create')
}
});
```

完成上述步骤后，您的 emloyeeForm 组件将类似于下面的代码片段。

员工表单

## 使用 axios 获取数据

创建一个名为 employeeList 的功能组件，它将获取和列出数据。获取简单直接，react-query 提供了一个名为 **useQuery** 的钩子，它接受一个负责获取数据的键和函数。从 react-query 导入名为 useQuery 的钩子。

```
import { useQuery } from 'react-query';
```

创建一个函数，负责从一个名为 restapiexample 的伪 rest api 获取数据。了解[**restaper 示例**](http://dummy.restapiexample.com/) **。**

```
const getEmployee = async () => {const { data } = await axios.get('http://dummy.restapiexample.com');return data.data;};
```

析构由钩子返回的数据，以及从 useQuery 钩子返回的其他几个数据。useQuery 提供了几个选项供选择。了解 [**使用查询选项**](https://react-query.tanstack.com/reference/useQuery#_top) **。**

```
const { data } = useQuery('create', getEmployee);
```

完整的 employeeList 应该类似于下面的代码片段。

查看一下 git[***repo***](https://github.com/Big-Zude/post-and-fetching-with-react-query.git)的其他配置，我们没有重点讨论，但是在文章中出现了。欢迎在推特[](https://twitter.com/Big_Zude)**上问我任何问题，我可以帮助你开始。**