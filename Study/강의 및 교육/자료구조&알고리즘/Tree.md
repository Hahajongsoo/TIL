# Tree
상당히 많이 그리고 여러 형태로 사용되는 자료구조이다. 거꾸로 되어있는 나무 모양을 생각하면 되고 그에 따라 시작점인 root와 나뉘는 선들인 branch와 끝 부분인 leaf로 구성되어 있다. node와 edge로 구성되어있는 형태로 그래프의 일종이다. 시작점인 루트 노드부터 시작하여 상위 노드에 연결된 하위 노드들의 관계로 표현된다. 상위 노드를 parent라고 부르고 하위 노드를 child라고 부른다. 같은 depth에 있는 노드들을 sibling이라고 한다. 각 노드는 자식노드를 가질 수 있고 자식노드가 없는 노드를 leaf 노드라고 한다. 

## 구현

```go
type TreeNode[T any] struct {
	Value T
	Childs []*TreeNode[T]
}
```
- 대표적으로 파일시스템이 트리의 일종이다. 
- 데이터를 표현할 때 계층이 있는 경우에 트리를 사용하는 경우가 많다.  
- 이외에도 binary tree, heap 등 데이터를 관리, 탐색하는데 유리한 구조들 에도 사용된다. 

## 순회
traversal, iterate등의 용어가 있으나 의미가 다르다. iterate의 경우 배열처럼 인덱스가 존재하는 자료구조에서 차례대로, 순서대로 탐색하는 것을 의미한다. traversal의 경우 순서가 존재하지 않는 그래프, 트리에서 탐색하는 것으로 탐색하는 순서에 따라 방법이 달라질 수 있다.

- 깊이 우선 탐색 (Depth First Search)
	- Inorder: 자식이 좌, 우만 있는 경우에 사용(이진 트리)
	- Preorder: 자신부터 먼저 탐색
	- Postorder: 자식부터 먼저 탐색
- 너비 우선 탐색 (Breadth First Search)

![](images/Pasted%20image%2020230615091506.png)
### 깊이 우선 탐색
- Preorder의 경우 A - B - E - F - C - D - G
- Postorder의 경우 E - F - B - C - G - D - A

```go
func (t *TreeNode[T]) Preorder(fn func(value T)) {
	if t == nil {
		return
	}
	fn(t.Value)

	for _, child := range t.Childs {
		child.Preorder(fn)
	}
}

func (t *TreeNode[T]) Postorder(fn func(value T)) {
	if t == nil {
		return
	}
	
	for _, child := range t.Childs {
		child.Postorder(fn)
	}
	fn(t.Value)
}

func (t *TreeNode[T]) DFS(fn func(value T)) {
	stack := make([]*TreeNode[T], 0)
	stack = append(stack, t)

	for len(stack) > 0 {
		last := stack[len(stack)-1]
		stack = stack[:len(stack)-1]

		fn(last.Value)

		stack = append(stack, last.Childs...)
	}
}
```

### 너비 우선 탐색
- A - B - C - D - E - F - G
```go
func (t *TreeNode[T]) BFS(fn func(value T)) {
	queue := make([]*TreeNode[T], 0)
	queue = append(queue, t)

	for len(queue) > 0 {
		front := queue[0]
		queue = queue[1:]

		fn(front.Value)

		queue = append(queue, front.Childs...)
	}
}
```

## gonum/plot 사용

깊이인 y 좌표는 preorder로 너비인 x 좌표는 inorder로 해결한다.


