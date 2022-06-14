# 广度优先搜索



## 基本框架
计算起点节点`start`到目标节点`target`的最短距离

```cpp
int bfs(Node start, Node target) {
	queue<Node> que;  // 节点访问队列
	unordered_map<Node, bool> visited;  // 记录已访问节点，可以用合适的数据结构代替

	que.push(start);
	visited[Node] = true;
	int step = 0; // 记录步数

	while(!que.empty()){  // 按层向外扩展
		int size = que.size(); // 记录当前层的节点数
		while(size--) {
			Noce cur = que.front();
			que.pop(); // 弹出队首节点
			if(cur == target) {
				return step;  // 到达终点
			}
			for (Node x : cur.adj()) { // 将所有未访问过的相邻节点加入队列
				if(!visited[x]) {
					que.push(x);
					visited[x] = true;
				}
			}
		}
		step++;  // 访问完一层，step+1
	}
}
```

## 题目

- [0111-二叉树的最小深度](_source/DSNA/lc0111.md)
- [0752-打开转盘锁](_source/DSNA/lc0752.md)