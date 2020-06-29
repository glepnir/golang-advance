## CircularLinkedList 环形链表

环形链表可以是单链表也可以是双链表，单链表的最后一个节点的 nextNode 是指向 nil 的，如果将它指向首节点
那么久形成了一个简单的环形链表，对于双向链表将首节点的 prevNode 指向最后一个节点，将最后一个节点的
nextNode 指向首节点，也就形成了环形的双向链表。

## 环形单链表

首先定义一个环形单链表,这里和第一节的单链表定义并无区别。

```go

// 节点
type Node struct {
	property int
	nextNode *Node
}

type CircularSingleList struct {
	len int
	// 环形单链表的首节点
	headNode *Node
}

func NewCircularSingleList() *CircularSingleList {
	return &CircularSingleList{}
}
```

### AddToHead 添加到头部的方法

- 大概的思路是和单链表类似的，区别在于添加之后我们需要找到最后一个节点将最后一个节点的 nextNode 指针
- 指向新插入的头结点。

```GO
func (c *CircularSingleList) AddToHead(property int) {
	node := &Node{property: property}
	if c.len == 0 {
		c.headNode = node
		c.len++
	} else {
		node.nextNode = c.headNode
		c.headNode = node
    // 区别在于这里需要找到最后一个节点
		lastNode := new(Node)
		for lastNode = c.headNode; ; lastNode = lastNode.nextNode {
      // 判断的条件就是一个节点的时候nextNode是nil 2个以上时最后一个节点的 nextNode
      // 是指向当前头节点
			if lastNode.nextNode == nil || lastNode.nextNode == c.headNode {
				break
			}
		}
		lastNode.nextNode = node
		c.len++
	}
}
```

- 输出

```go
func main() {
	// 初始化一个空的环形单链表
	c := NewCircularSingleList()
	// 添加到头部 此时的环形单链表首节点是7
	c.AddToHead(7)
	// 再添加一个节点到头部 那么此时的首节点是8
	c.AddToHead(8)
	// 打印当前的首节点 输出8
	fmt.Println(c.headNode.property)
	// 打印当前的环形单链表的第二个节点也是当前环形链表的最后一个节点应该是7
	fmt.Println(c.headNode.nextNode.property) // 输出7
	// 打印最后一个节点的下一个节点的指向 应该是指向首节点也就是8
	fmt.Println(c.headNode.nextNode.nextNode.property) // 输出8
}
```

### LastNode 查找最后一个节点的方法

- 直接从 AddToHead 中抽取出来即可。

```GO
func (c *CircularSingleList) LastNode() *Node {
	node := new(Node)
	if c.len == 0 {
		return nil
	}
	for node = c.headNode; ; node = node.nextNode {
		if node.nextNode == nil || node.nextNode == c.headNode {
			break
		}
	}
	return node
}
```

### AddToEnd 尾部增加的方法

- 通过 LastNode 方法获取到最后一个节点，因为这个新节点是要放到最后的，所以这个新节点的 nextNode
- 一定是指向头结点的直接赋值就可以了。然后将当前的 lastNode 的 nextNode 指向这个新节点

```go
func (c *CircularSingleList) AddToEnd(property int) {
	node := &Node{property: property}
	lastNode := c.LastNode()
	node.nextNode = c.headNode
	lastNode.nextNode = node
}
```

### 输出

```GO
func main() {
	// 初始化一个空的环形单链表
	c := NewCircularSingleList()
	// 添加到头部 此时的环形单链表首节点是7
	c.AddToHead(7)
	// 再添加一个节点到头部 那么此时的首节点是8
	c.AddToHead(8)
	// 打印当前的首节点 输出8
	fmt.Println(c.headNode.property)
	// 打印当前的环形单链表的第二个节点也是当前环形链表的最后一个节点应该是7
	fmt.Println(c.headNode.nextNode.property) // 输出7
	// 打印最后一个节点的下一个节点的指向 应该是指向首节点也就是8
	fmt.Println(c.headNode.nextNode.nextNode.property) // 输出8
	c.AddToEnd(4)
	fmt.Println(c.LastNode().property) // 输出4
}
```