# Standard Template Library

## Containers

    vector list stack queue deque priority_queue 
      bitset array
    forward_list 单向链表 string pair<,>
    
    set multiset map multimap unordered_map unordered_multimap unordered_set unordered_multiset

## Iterators

    iterator reverse_iterator bidirectional_iterator forward_iterator random_access_iterator

    auto .begin() .end()
    const_iterator .cbegin() .cend() 常量迭代器

    Type of iterator: ContainerType::iterator

## Functions

    .size()  .empty()
    .resize(num) resize to num elements
    .resize(num,x) resize to num elements, fill with x
    .clear() clear all elements

### 构造函数

| Function | Description |
| --- | --- |
|make_pair() |创建pair|
|containerType c(num) |创建容器，元素个数为num|
|containerType c(num, val) |创建容器，元素个数为num，每个元素的值为val|
|containerType c(first, last) |创建容器，元素个数为last-first，元素的值为[first, last)区间内的值|

### 迭代函数

| Function | Description |
| --- | --- |
|advance(iterator, n)| 迭代n次|
|distance(iterator, iterator) |返回两个迭代器之间的距离|
|next(iterator, n) |返回迭代器的下一个迭代器|
|prev(iterator, n) |返回迭代器的上一个迭代器|

### detail

1. stack
   Container Adapter based on Sequence Container

2. initialize_list
   可以一次赋值 不用每个push_back() 例如：`cpp std::vector v = { 1, 2, 3, 4 };`

3. emplace()
   在插入元素时，是在容器的指定位置直接构造元素，而不是先单独生成，再将其复制（或移动）到容器中。
   因此，在实际使用中，推荐大家优先使用 emplace() 函数原型：emplace(pos,val)

4. vector
    每次申请两倍空间，加入一个数平均时间是O(1)
    capacity() 返回容器的容量 (申请空间)
    reserve() 调整空间
    删除元素后会回收空间，不是每次回收 删除到原来的1/4 会回收 复杂度证明：势能分析

5. list
    reverse() 将整个链表翻转，交换prev,next，首尾特别处理
    forward_list::reverse() ? 一个一个来，将可能会破坏的指向关系提前保存

6. priority_queue
    重载operator<
    默认是大顶堆，可以重载operator>实现小顶堆

### Others

    sort()  lower_bound() upper_bound()  merge()(二路归并)

### Reference

    cppreference.com
