# 数组

## 二分查找

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left <= right){
            int mid = (right - left) /2 +left;
            int num = nums[mid];
            if(num == target){
                return mid;
            }else if(num>target){
                right = mid-1;
            }else{
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

## 移除元素

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slowIndex = 0;
        for(int fastIndex=0;fastIndex<nums.length;fastIndex++){
            if(val!=nums[fastIndex]){
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
}
```

## 有序数组的平方

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int index = nums.length - 1;
        int[] result = new int[nums.length];
        while(left <= right){
            if(nums[left] * nums[left] > nums[right] * nums[right]){
                result[index--] = nums[left] * nums[left];
                left++;
            }else{
                result[index--] = nums[right] * nums[right];
                right--;
            }
        }
        return result;
    }
}
```

## 长度最小的子数组

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for(int right = 0;right < nums.length; right++){
            sum += nums[right];
            while(sum >= target){
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];
            }
        }
        return result ==  Integer.MAX_VALUE ? 0 : result;
    }
}
```

## 螺旋数组

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int index = 0;
        int[][] result = new int[n][n];
        int start = 0;
        int count = 1;
        int i,j;

        while(index++ < n/2){
            for(j=start;j<n-index;j++){
                result[start][j] = count++;
            }
            for(i=start;i<n-index;i++){
                result[i][j] = count++;
            }
            for(;j>=index;j--){
                result[i][j] = count++;
            }
            for(;i>=index;i--){
                result[i][j] = count++;
            }
            start++;
        }
        if(n%2==1){
            result[start][start] = count;
        }
        return result;
    }
}
```

# 链表

## 移除链表元素

### 循环

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if(head == null){
            return head;
        }
        ListNode index = new ListNode(-1, head);
        ListNode pre = index;
        ListNode current = head;
        while(current != null){
            if(current.val == val){
                pre.next = current.next;
            }else{
                pre = current;
            }
            current = current.next;
        }
        return index.next;
    }
}
```

### 递归

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
       if(head==null)
           return null;
        head.next=removeElements(head.next,val);
        if(head.val==val){
            return head.next;
        }else{
            return head;
        }
    }
}
```

## 设计链表

### 单链表

```java
class ListNode{
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val){
        this.val=val;
    }
}
class MyLinkedList {
    // size存储链表元素的个数
    int size;
    // 虚拟头结点
    ListNode head;
    public MyLinkedList() {
        size=0;
        head = new ListNode(0);
    }
    
    public int get(int index) {
        if(index<0||index>=size){
            return -1;
        }
        ListNode current = head;
        for(int i=0;i<=index;i++){
            current = current.next;
        }
        return current.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0,val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index>size){
            return;
        }
        if(index<0){
            index=0;
        }
        size++;
        ListNode pred = head;
        for(int i=0;i<index;i++){
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }
    
    public void deleteAtIndex(int index) {
        if(index<0 || index >= size){
            return;
        }
        size--;
        if(index == 0){
            head = head.next;
            return;
        }
        ListNode pred = head;
        for(int i=0;i<index;i++){
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}
```

### 双链表

```java
class ListNode{
    int val;
    ListNode next,prev;
    ListNode() {};
    ListNode(int val){
        this.val = val;
    }
}


class MyLinkedList {  

    //记录链表中元素的数量
    int size;
    //记录链表的虚拟头结点和尾结点
    ListNode head,tail;
    
    public MyLinkedList() {
        //初始化操作
        this.size = 0;
        this.head = new ListNode(0);
        this.tail = new ListNode(0);
        //这一步非常关键，否则在加入头结点的操作中会出现null.next的错误！！！
        head.next=tail;
        tail.prev=head;
    }
    
    public int get(int index) {
        //判断index是否有效
        if(index<0 || index>=size){
            return -1;
        }
        ListNode cur = this.head;
        //判断是哪一边遍历时间更短
        if(index >= size / 2){
            //tail开始
            cur = tail;
            for(int i=0; i< size-index; i++){
                cur = cur.prev;
            }
        }else{
            for(int i=0; i<= index; i++){
                cur = cur.next; 
            }
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        //等价于在第0个元素前添加
        addAtIndex(0,val);
    }
    
    public void addAtTail(int val) {
        //等价于在最后一个元素(null)前添加
        addAtIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        //index大于链表长度
        if(index>size){
            return;
        }
        //index小于0
        if(index<0){
            index = 0;
        }
        size++;
        //找到前驱
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        //新建结点
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next.prev = newNode;
        newNode.prev = pre;
        pre.next = newNode;
        
    }
    
    public void deleteAtIndex(int index) {
        //判断索引是否有效
        if(index<0 || index>=size){
            return;
        }
        //删除操作
        size--;
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        pre.next.next.prev = pre;
        pre.next = pre.next.next;
    }
}
```

## 反转链表

### 双指针

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        ListNode temp = null;
        while(cur != null){
            temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

### 递归

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }

    private ListNode reverse(ListNode prev, ListNode cur) {
        if (cur == null) {
            return prev;
        }
        ListNode temp = null;
        temp = cur.next;// 先保存下一个节点
        cur.next = prev;// 反转
        // 更新prev、cur位置
        // prev = cur;
        // cur = temp;
        return reverse(cur, temp);
    }
}
```

### 从后往前递归

```java
class Solution {
    ListNode reverseList(ListNode head) {
        // 边缘条件判断
        if(head == null) return null;
        if (head.next == null) return head;
        
        // 递归调用，翻转第二个节点开始往后的链表
        ListNode last = reverseList(head.next);
        // 翻转头节点与第二个节点的指向
        head.next.next = head;
        // 此时的 head 节点为尾节点，next 需要指向 NULL
        head.next = null;
        return last;
    } 
}
```

