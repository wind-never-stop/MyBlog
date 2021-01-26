---
title: RecyclerView 局部刷新，指定刷新对应item的控件
categories: 
- Android
- 控件
tags:
- Android
- RecyclerView
---
可以使用

``` Kotlin
mAdapter.notifyItemChanged(index,1) 
```

第一个参数 index 是刷新哪个item <br> 第二个参数是哪个item里的哪些控件需要刷新的判断值需要我们自己处理，或者不传刷新整个item；

<!--more-->

在 Adapter 中复写  
```Kotlin
onBindViewHolder(
        holder: BaseViewHolder,
        position: Int,
        payloads: MutableList<Any>
    ) 
```

该方法如下：
```Kotlin
    override fun onBindViewHolder(
        holder: BaseViewHolder,
        position: Int,
        payloads: MutableList<Any>
    ) {
       if (payloads.isEmpty()){
           onBindViewHolder(holder,position)
       }else{
           super.onBindViewHolder(holder, position)
       }
    }
```
payloads就是我们在刷新 该item是传入的第二个参数
根据  payloads 参数进行判断需要刷新那些控件；如果该参数为null哪么就需要刷新整个item


在某些情况下recycerView刷新会有闪烁的情况  我们只需要禁止recycleView的动画就好了 如下：
```Kotlin
 (swipeRecyclerView?.getItemAnimator() as DefaultItemAnimator).setSupportsChangeAnimations(false)
```

还有一种是设置动画播放时间为0；
如下：
```Kotlin
swipeRecyclerView?.getItemAnimator()?.setChangeDuration(0);// 通过设置动画执行时间为0来解决闪烁问题
```
不过不推荐使用这种方法，因为在某些情况刷新的item下一个item会晃动一下。