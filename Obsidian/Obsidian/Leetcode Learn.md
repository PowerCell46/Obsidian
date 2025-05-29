
Two pointer sort

```c++
class Solution {  
public:  
    vector<int> sortedSquares(vector<int>& nums) {  
        int n = nums.size();  
        vector<int> result(n);  
        int left = 0, right = n - 1, index = n - 1;  
  
        while (left <= right) {  
            int leftSquare = nums[left] * nums[left];  
            int rightSquare = nums[right] * nums[right];  
  
            if (leftSquare > rightSquare) {  
                result[index--] = leftSquare;  
                ++left;  
            } else {  
                result[index--] = rightSquare;  
                --right;  
            }        }  
        return result;  
    }};
```

### `vector<int> result(n);`

**creates a vector named `result` with `n` elements, each initialized to `0`** (default initialization for `int` in a vector).

Unlike `vector<int> result;`, which starts empty and grows dynamically, `vector<int> result(n);` pre-allocates memory for `n` integers, which avoids the overhead of dynamic reallocation.

### vector<int> result(n, -1); // Creates: {-1, -1, -1, -1, -1}
