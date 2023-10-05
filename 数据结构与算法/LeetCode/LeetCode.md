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

