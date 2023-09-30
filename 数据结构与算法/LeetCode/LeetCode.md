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
```



