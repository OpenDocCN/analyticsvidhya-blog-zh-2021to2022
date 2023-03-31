# ES6 Javascript 对象和数组 II

> 原文：<https://medium.com/analytics-vidhya/es6-javascript-object-and-array-2-f8c81d67c722?source=collection_archive---------20----------------------->

![](img/6647db124322ee065736e86c6a398e51.png)

斯洛伐克斯皮斯城堡

在这篇文章中，我们将主要关注那些内置数组函数和几个用例。

1.  减少()

对数组中的每个元素执行函数，并将它们累加，得到最终结果

```
Example 1let numbers = [1, 2, 3, 4, 5];let result = numbers.reduce(sum);//total - the value of last result
//At the beginning, the total is 1 - 1st element in array, 
//and num is 2 - 2nd element array//Then the value of total will be 1 + 2 = 3, 
//and num will be 3 - 3rd element array//Then the value of total will be 1 + 2 + 3 = 6, 
//and num will be 4 - 4th element array//Then the value of total will be 1 + 2 + 3 + 4 = 10, 
//and num will be 5 - 5th element array//Then the value of total will be 1 + 2 + 3 + 4 + 5 = 15,
//becoz this array got 5 elements only, so 15 will be the final //answerfunction sum(total, num) {
  return total + num;
}console.log(result);
//output 15, becoz 1 + 2 + 3 + 4 + 5
```

另一个例子

```
let numbers = [100, 40, 30, 20, 10];let result = numbers.reduce(minus);function minus(total, num) {
  return total - num;
}console.log(result);
//output 0, becoz 100 - 40 - 30 - 20 - 10
```

另一个例子(初始值有可选参数)

```
let numbers = [100, 40, 30, 20, 10];//300 is the initial value we given, 
//optional parameter of reduce function
let result = numbers.reduce(minus, 300);//total - the value of last result
//At the beginning, the total is the initial value we provide - 300
//and num is 100 - 2nd element array//Then the value of total will be 300 - 100 = 200, 
//and num will be 30 - 3rd element array//Then the value of total will be 300 - 100 - 40 = 160, 
//and num will be 20 - 4th element array//Then the value of total will be 300 - 100 - 40 - 20 = 110, 
//and num will be 10 - 5th element array//Then the value of total will be 300 - 100 - 40 - 20 - 10 = 100,
//becoz this array got 5 elements only, so 100 will be the final //answerfunction minus(total, num) {
  return total - num;
}console.log(result);
//output 100, becoz 300 - 100 - 40 - 30 - 20 - 10
```

2.过滤器()

返回一个**新数组**,其中包含那些通过了过滤函数的元素(返回 true)

```
let numbers = [100, 40, 30, 20, 10];let result = numbers.filter(checking);//filter function, check whether number smaller than 100
function checking(num) {
  return num < 100;
}console.log(result);
//[40, 30, 20, 10]
```

另一个例子

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];let result = fruits.filter(checking);//check whether each fruit contains character 'i'
function checking(fruit) {
  return fruit.includes('i');
}console.log(result);
//['kiwi']
//only 'kiwi' contains 'i'
```

3.拼接()

在现有数组中添加或移除元素，并返回包含移除元素的新数组

移除元素示例—从第一个元素开始

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//2 parameters, the first one is which element by index to remove
//the second parameter is how many element to be remove
//In this example, start from index 0 to remove 1 element
let removedElements = fruits.splice(0, 1);console.log(fruits);
//['orange', 'banana', 'kiwi']console.log(removedElements);
//['apple']
```

另一个关于移除元素的例子——从最后一个元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//Let say we want to started from the last element, we can use //negative number in the first parameter
//the last element is -1, vice versa
//And the direction (of removing) is same as positive number //parameter
//So, 'banana' and 'kiwi' will be remove instead of 'banana' and //'orange'let removedElements = fruits.splice(-2, 2);console.log(fruits);
//['apple', 'orange']
//-2 means the second last element, that means removed 'banana'console.log(removedElements);
//['banana', 'kiwi']
```

添加元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//first 0, start from index 0
//second 0, remove 0 element
//'mango' - the new element we going to add, it will add to the //first element (according to first param)fruits.splice(0, 0, 'mango');console.log(fruits);
//['mango', 'apple', 'orange', 'banana', 'kiwi']
```

添加和删除元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//first 1, start from index 1 (second element)
//second 1, remove one element
//'mango', 'pineapple' - the new element we going to addfruits.splice(1, 1, 'mango', pineapple);console.log(fruits);
//['apple', 'banana', 'kiwi', 'orange', 'banana', 'kiwi']
```

4.切片()

按起始和结束索引将所选元素复制到新数组

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//first parameter, start from index 1 (second element)
//second parameter, end at index 3(fourth element), but not //including itvar someFruits = fruits.slice(1, 3);console.log(someFruits);
//['orange', 'banana']
```

另一个例子，使用负指数

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//first parameter, start from index -3 (second element)
//second parameter, end at index 3(fourth element), but not //including it
//And the direction (of removing) is same as positive number //parameter
//So the second parameter always larger than the first parameter
//In this case, the last 3rd element is 'orange' and end at the last //2nd element (but not including it), so only 'orange' will be //copyvar someFruits = fruits.slice(-3, -2);console.log(someFruits);
//['orange']
```

另一个例子，没有参数

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];//by default, if we left empty, 1st parameter will be 0
//for the 2nd parameter, if we left empty, will be the end of array
//that means in this case we will copy everything from the array
var someFruits = fruits.slice();console.log(someFruits);
//['apple', 'orange', 'banana', 'kiwi']
```

5.concat()

将其他数组的元素追加到末尾，并在新数组中返回新结果。

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];let snacks = ['chips', 'candy', 'chocolate', 'mints'];//always returns new array, existing arrays not affected
let food = fruits.concat(snacks);console.log(food);
//['apple', 'orange', 'banana', 'kiwi', 'chips', 'candy', 'chocolate', 'mints']
```

6.查找()

与 filter()类似，但是一旦找到第一个返回，它就停止搜索

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];let result = fruits.find(findFunc);//filter function, check whether number smaller than 100
function findFunc(fruit) {
  return fruit.includes('a');
}console.log(result);
//apple
//apple, orange, banana also contain 'a', but since 'apple' locate //at the first index, so return 'apple'
```

7.findIndex()

与 find()类似，从每个数组元素开始搜索，一旦找到第一个返回就停止搜索，但是返回索引号而不是值

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];let result = fruits.findIndex(findFunc);//filter function, check whether number smaller than 100
function findFunc(fruit) {
  return fruit.includes('a');
}console.log(result);
//0
//As apple located at the first index
```

8.对象.分配()

将属性从一个或多个对象复制到另一个对象。不会返回新数组，只会添加/覆盖目标对象(第一个参数)

```
const fruitsCount = { kiwi: 1, banana: 2 };
const snacksCount = { candy: 4, chocolate: 9 };//1st parameter is the target object, all properties in 2nd and
//other parameter will be copy to the 1st parameter
//In this case, the properties from snacksCount will be copy to //fruitsCountObject.assign(fruitsCount, snacksCount);console.log(fruitsCount);
// {kiwi: 1, banana: 2, candy: 4, chocolate: 9}console.log(snacksCount);
// {candy: 4, chocolate: 9}
```

另一个例子

```
const fruitsCount = { kiwi: 1, banana: 2 };
const snacksCount = { candy: 4, chocolate: 9, banana: 3 };
const vegeCount = { eggPlant: 7, banana: 5};//1st parameter is the target object, all properties in 2nd and
//other parameter will be copy to the 1st parameter
//In this case, the properties from snacksCount and vegeCount will //be copy to fruitsCount
//If multi parameter got the same properties ('banana' in this //case), then the properties from "later" parameter will be apply
//In this case, 'banana: 5' from vegeCount will apply instead of //'banana: 3' from snacksCountObject.assign(fruitsCount, snacksCount, vegeCount);console.log(fruitsCount);
{kiwi: 1, banana: 5, candy: 4, chocolate: 9, eggPlant: 7}console.log(snacksCount);
// {candy: 4, chocolate: 9, banana: 3}console.log(vegeCount);
// {eggPlant: 7, banana: 5}
```

9.索引 Of

通过元素值查找数组中的索引，如果不止一个数组元素得到相同的值，那么它将返回第一个元素的索引

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];let index = fruits.indexOf('orange');console.log(index);
//return 1
```

10.推

向现有数组的最后一个索引添加新元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];fruits.push('cherry');console.log(fruits);
//['apple', 'orange', 'banana', 'kiwi', 'cherry']
```

11.流行音乐

从现有数组中删除最后一个元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];fruits.pop();console.log(fruits);
//['apple', 'orange', 'banana']
```

12.变化

从现有数组中移除第一个元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];fruits.shift();console.log(fruits);
//['orange', 'banana', 'kiwi']
```

13.松开打字机或键盘的字型变换键

向数组的第一个元素添加新元素

```
let fruits = ['apple', 'orange', 'banana', 'kiwi'];fruits.shift('cherry');console.log(fruits);
//['cherry', 'apple', 'orange', 'banana', 'kiwi']
```

一些常见的使用案例

1.  删除重复元素

按过滤器()

```
let duplicate_array = ['apple', 'orange', 'orange', 'banana', 'banana', 'kiwi', 'banana'];let filtered_array = duplicate_array.filter(function(element, index) {return index === duplicate_array.indexOf(element); })console.log(filtered_array);
//['apple', 'orange', 'banana', 'kiwi']
```

通过 Array.from()和 Set

```
let duplicate_array = ['apple', 'orange', 'orange', 'banana', 'banana', 'kiwi', 'banana'];//Automatically removes duplicate elements
const set = new Set(duplicate_array);let filtered_array = Array.from(set);console.log(filtered_array);
//['apple', 'orange', 'banana', 'kiwi']
```

2.查找重复元素

通过 reduce()和 indexOf()

```
let duplicate_array = ['apple', 'orange', 'orange', 'banana', 'banana', 'kiwi', 'banana'];function sum(list, element, index) {//just keep in mind that the default index is 1, becoz the total is //the value of 1st element. However, if we added the optional //parameter - initial value, the index will be 0 (the 1st index in //array)
let duplicated = index !== duplicate_array.indexOf(element);if (duplicated && list.indexOf(element) < 0)    {
        list[list.length] = element
    }return list;    
}let duplicated_list = duplicate_array.reduce(sum, []);console.log(duplicated_list);
//['orange', 'banana']
```

3.用嵌套数组复制数组

随着国家管理层的反应。JS 和 Redux 总是需要不可变的状态值，所以我们通常不更新当前对象，而是创建新的对象并复制值，以便符合标准。(关于不可变状态的原因，请参考[的另一段话](/analytics-vidhya/reactjs-in-simple-english-state-7aecb126a29b)

[关于以不可变方式更新 Redux 状态的更多信息](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns)-[https://Redux . js . org/recipes/structuring-reducers/immutable-update-patterns](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns)

```
const fruits = ['kiwi', 'banana'];
const snacks = ['candy', 'chocolate'];
const drink = ['beer', 'coke'];let food = [fruits, snacks];
let all = [food, drink];//only copy the reference address
let copy_all = allall.push('new_item');
//both 'all' and 'copy_all' show the new value, that means we copied //the object reference only
console.log(all);
console.log(copy_all);copy_all = [..all];
all.push('new_item2');
//array 'all' reflects new value, array 'copy_all' doesn't, that //means we copied the value, not reference
console.log(all);
console.log(copy_all);//try to add new value 'new_item3' to the nested array
all[0].push('new_item3');
//both 'all' and 'copy_all' show the new value, that means for the //nested array we copied the object reference only//we realize that each array/ nested array have to be copied //'manually'//the final result
copy_all = [[[...all[0][0]], [...all[0][1]]], [...all[1]]];
console.log(copy_all);
```

下面的数组内置函数会编辑现有的数组，所以在编辑 React 的状态时可以避免使用它们。JS / Redux

**splice()
object . assign()
push()
pop()
shift()
un shift()**