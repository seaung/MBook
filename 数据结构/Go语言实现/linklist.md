##### 链表概念

链表由一系列不连续节点组成的序列。每个结构均含有表示表元素和指向包含该元素后继元的结构指针。

##### Golang链表结构

```go
type Node struct {
    propetry int
    netxNode *Nonde
}

// 带头节点的链表
type LinkNode struct {
    headNode *Node
}
// 使用headNode作为头指针遍历下个节点
```

##### 链表的常见操作

1. 添加操作

```go
func (l *LinkNode)AppendToHead(propetry int) {
    var node = Node{}
    node.propetry = propetry
    if node.nextNode != nil {
        node.nextNode = l.headNode
    }
    l.headNode = &node
}

func main() {
    var link LinkNode
    link = &LinkNode{}
    link.AppendToHead(1)
    fmt.Println(link.headNode.propetry)
}
```

AppendToHead方法是将节点添加到链表的开头。该方法接受一个整型的属性参数，该参数用于初始化节点。一个新的节点被实例化，其属性被设置为我们传递的属性值。nextNode指向LinkNode的当前headNode,headNode设置为新创建的节点的指针
