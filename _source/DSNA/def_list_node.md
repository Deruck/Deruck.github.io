# 链表节点的定义

**c++实现**
```cpp
struct ListNode {
	// 成员：节点值、指向下一个节点的指针
	int val; 
	ListNode *next;

	// 构造函数
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}

    // 打印链表
    void print() {
        ListNode *ptr = this;
        while (!(ptr == nullptr)) {
            cout << ptr->val << " -> ";
            ptr = ptr->next;
        }
        cout << "null" << endl;
    }
};

```

**使用**
```cpp
// 使用默认构造函数
ListNode *head0 = new ListNode(0);
head0->print();

// 使用节点值构造一个指向空节点的节点
ListNode *head5 = new ListNode(5);
head5->print();

// 初始化下一个节点，连接在head5上
head5->next = new ListNode(6);
head5->print();

// 使用节点值和下一个节点地址构造节点
ListNode *head7 = new ListNode(7, head5);
head7->print();
```
**out**
```
0 -> null
5 -> null
5 -> 6 -> null     
7 -> 5 -> 6 -> null
```