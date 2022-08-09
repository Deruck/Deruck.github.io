# 二叉树



## 层序遍历
### 模板
#### 无需记录层  
```cpp
void traverse(TreeNode* root) {
	if(root == nullptr) {
		return;
	}
	queue<TreeNode*> nodeQue;
	nodeQue.push(root);
	while(!nodeQue.empty()) {
		// 节点操作逻辑
		manipulate(nodeQue.front());
		// 将左右子节点加入队列
		if(nodeQue.front()->left != nullptr) 
			nodeQue.push(nodeQue.front()->left);
		if(nodeQue.front()->right != nullptr) 
			nodeQue.push(nodeQue.front()->right);
		nodeQue.pop();
	}
}
```

#### 需要记录层  
```cpp
void traverse(TreeNode* root) {
	if(root == nullptr) {
		return;
	}
	queue<TreeNode*> nodeQue;
	nodeQue.push(root);
	while(!nodeQue.empty()) {
		// 记录当前层节点总数
		int step = nodeQue.size();
		while(step--) {
			// 节点操作逻辑
			manipulate(nodeQue.front());
			// 将左右子节点加入队列
			if(nodeQue.front()->left != nullptr) 
				nodeQue.push(nodeQue.front()->left);
			if(nodeQue.front()->right != nullptr) 
				nodeQue.push(nodeQue.front()->right);
			nodeQue.pop();
		}
		// 层操作逻辑
		manipulateLayer();
	}
}
```

### 题目

- [0116-填充每个节点的下一个右侧节点指针](_source/DSNA/lc0116.md)

## 前序遍历
### 模板
#### 递归
```cpp
void traverse(TreeNode* root) {
	// 递归出口
	if(root == nullptr) {
		return;
	}
	// 节点操作逻辑
	manipulate(root);

	// 递归调用
	traverse(root->left);
	traverse(root->right);
}
```

#### 栈
```cpp
void traverse(TreeNode* root) {
	if(root == nullptr) {
		return;
	}
	stack<TreeNode*> nodeStk;
	nodeStk.push(root);
	while(!nodeStk.empty()) {
		TreeNode* top = nodeStk.top();
		nodeStk.pop();
		
		// 节点操作逻辑
		manipulate(top);

		// 将左右节点压入栈(右节点先入，后访问)
		if(top->right != nullptr) nodeStk.push(top->right);
		if(top->left != nullptr) nodeStk.push(top->left);
	}
}
```

### 题目
- [0105-从前序与中序遍历序列构造二叉树](_source/DSNA/lc0105.md)
- [0114-二叉树展开为链表](_source/DSNA/lc0114.md)
- [0297-二叉树的序列化与反序列化](_source/DSNA/lc0297.md)
- [0654-最大二叉树](_source/DSNA/lc0654.md)

## 后序遍历

将前序遍历替换顺序即可，特点为先操作子树再操作根节点，可以获得子树信息，一般在子树问题中用到。

- [0652-寻找重复的子树](_source/DSNA/lc0652.md)


## 二叉树构造

通过遍历序列构造二叉树，通过两种遍历方式的空间特点，定位根节点位置，继而推算出左右子树的位置，然后通过构造左右子树递归地构造出当前子树。
- [0105-从前序与中序遍历序列构造二叉树](_source/DSNA/lc0105.md)
- [0106-从中序与后序遍历序列构造二叉树](_source/DSNA/lc0106.md)
- [0889-根据前序和后序遍历构造二叉树](_source/DSNA/lc0889.md)

通过完整的前序遍历构造二叉树
- [0297-二叉树的序列化与反序列化](_source/DSNA/lc0297.md)
## 汇总

- [0104-二叉树的最大深度](_source/DSNA/lc0104.md)
- [0105-从前序与中序遍历序列构造二叉树](_source/DSNA/lc0105.md)
- [0106-从中序与后序遍历序列构造二叉树](_source/DSNA/lc0106.md)
- [0111-二叉树的最小深度](_source/DSNA/lc0111.md)
- [0114-二叉树展开为链表](_source/DSNA/lc0114.md)
- [0116-填充每个节点的下一个右侧节点指针](_source/DSNA/lc0116.md)
- [0226-翻转二叉树](_source/DSNA/lc0226.md)
- [0297-二叉树的序列化与反序列化](_source/DSNA/lc0297.md)
- [0543-二叉树的直径](_source/DSNA/lc0543.md)
- [0652-寻找重复的子树](_source/DSNA/lc0652.md)
- [0654-最大二叉树](_source/DSNA/lc0654.md)
- [0889-根据前序和后序遍历构造二叉树](_source/DSNA/lc0889.md)