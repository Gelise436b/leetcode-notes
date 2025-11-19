
---

### [19] 删除链表的倒数第 N 个节点

#### 🔑 核心思路：快慢指针（双指针）

- **目标**：在单次遍历中定位倒数第 `N` 个节点的前驱，实现高效删除。
- **关键技巧**：
  - 引入**虚拟头节点 `dummy`**，统一处理删除头节点的边界情况。
  - **快指针 `right` 先走 `N` 步**，使快慢指针之间保持 `N` 个节点的距离。
  - 然后**快慢指针同步前进**，当 `right->next == nullptr`（即 `right` 到达尾节点）时，`left` 恰好停在待删节点的前驱位置。
- **删除操作**：
  ```cpp
  ListNode* nxt = left->next;
  left->next = left->next->next;
  delete nxt; // 避免内存泄漏（LeetCode 通常不强制，但良好习惯）
  ```

#### 💡 反思与提升

- **问题根源**：虽然熟悉数组/字符串中的双指针，但在链表场景下未能迁移应用，说明对“指针即位置”的抽象理解不够牢固。
- **通用模式**：  
  > “**快指针先行 K 步 → 同步移动 → 慢指针到达目标前驱**”  
  是解决“倒数第 K 个”类链表问题的标准范式，应形成条件反射。

#### ✅ 正确代码（带虚拟头 + 内存安全）

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode dummy(0, head);
        ListNode *left = &dummy, *right = &dummy;
        
        // 快指针先走 n 步
        while (n--) {
            right = right->next;
        }
        
        // 同步移动，直到快指针到达最后一个节点
        while (right->next) {
            left = left->next;
            right = right->next;
        }
        
        // 删除 left->next（即倒数第 n 个节点）
        ListNode* toDelete = left->next;
        left->next = left->next->next;
        delete toDelete;
        
        return dummy.next;
    }
};
```

#### 📌 总结

- 快慢指针不仅是数组技巧，更是**跨数据结构的通用策略**。
- 虚拟头节点能极大简化链表头操作的边界逻辑。
- 链表题的核心在于**指针关系的精准控制**，而非复杂算法。


--- 

如需我帮你生成 Markdown 文件或继续整理其他题目笔记，请随时告诉我！
