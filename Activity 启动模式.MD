# Activity 启动模式

FROM:
http://lizwangying.github.io/2016/08/21/Activity%E7%9A%84%E6%AD%A3%E7%A1%AE%E6%89%93%E5%BC%80%E6%96%B9%E5%BC%8F/

###standard
标准模式

###signleTop
对于signleTop Activity :

- 在栈顶: 不会新起 activity, 直接调用 onNewIntent 方法
- 不在栈顶: 和标准模式一样

###SingleInstance
必定会在一个新的 Task 之中, 并且这个 Task 之中有且只能有一个 Activity 

###signleTask
已新的栈启动 Activity, 需要和 taskAffinity 配合使用
对于signleTask Activity : 只有 taskAffinity 和 启动它的 activity 不同时才会在新的 Task 中启动

- A(Default) -> B(singleTask) : B taskAffinity = task2
- A(default) -> B(singleTask) -> C(singleTask) -> B(singleTask) : B&C taskAffinity = task2

```

if( 发现一个 Task 的 affinity == Activity 的 affinity ){
    if(此 Activity 的实例已经在这个 Task 中){
        这个 Activity 启动并且清除顶部的 Acitivity ，通过标识 CLEAR_TOP 
    } else {
        在这个 Task 中新建这个 Activity 实例
    }
} else { // Task 的 affinity 属性值与 Activity 不一样
    新建一个 affinity 属性值与之相等的 Task
    新建一个 Activity 的实例并且将其放入这个 Task 之中
}

```


比较以下启动流程:
A(Default) -> B(singleTask)

A(Default) -> B(singleTask) : B has a taskAffinity attribute

A(default) -> B(singleTask) -> C(singleTask) -> B(singleTask) : B&C has a taskAffinity attribute

A(default) –> B(singleInstance) –> C(default)

A(default) –> B(singleTask) –> C(default)


