---
title: ArrayList和LinkedList
categories: Java
---
简单分析一下ArrayList和LinkedList的区别和应用场景。
<!-- more -->
## ArrayList--本质是数组
ArrayList的实现是一个数组Array，数组是有下标来标识每一个元素的位置的，所以ArrayList的读取速度非常快，而在增删元素的时候，一旦涉及需要删除中间甚至更靠前的元素，被操作元素之后的所有元素的序号都需要进行重排(这里需要强调，如果增删的元素越靠后，所需要做的处理越少，如果仅仅是在数组末尾增加或删除元素，速度也是及其快的，因为根本不需要做多余的序号重排工作)。<br>
## LinkedList--本质是由节点串连的链表
LinkedList的实现有所不同，它使用Node的方式存储元素，每个Node包含该元素以及该元素对应的上个元素和下个元素的节点Node，源码如下：
```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```
而在读取元素的时候，因为元素没有编号，所以只能通过节点遍历查询来获取，当然，这里考虑效率问题，效率很低，离两端越近的节点的元素读取越快。源码如下：
```java
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
    //此处为移位运算，二进制右移一位表示除以2
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```
所以相对来说，在任何地方增删元素只需要改变该元素对应的上下节点就可以了，效率是一样快的，这也是LinkedList的优势所在。

然而大多数情况下，对集合的增删操作的要求并不高，而且大多只是在末尾增删，所以ArrayList使用的频率要比LinkedList高很多。

### LinkedList的适用场景需满足：
1. 你的应用不会随机访问数据。因为如果你需要LinkedList中的第n个元素的时候，你需要从第一个元素顺序数到第n个数据，然后读取数据。<br>
2. 你的应用更多的插入和删除元素，更少的读取数据。因为插入和删除元素不涉及重排数据，所以它要比ArrayList要快。




