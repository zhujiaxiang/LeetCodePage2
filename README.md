## LeetCodePage2
### 350. Intersection of Two Arrays II
Given two arrays, write a function to compute their intersection.

#### Example:

```
Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2, 2].
```


#### Note:
Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.
#### Follow up:
What if the given array is already sorted? How would you optimize your algorithm?

What if nums1's size is small compared to nums2's size? Which algorithm is better?

What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?
#### 问题
求出两个数组中全部重复的元素
#### 思路
使用unordered_map 进行统计++，随后再比较--
#### C++

```
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    
    unordered_map<int, int> map;
    vector<int> res;
    for (int i=0; i<nums1.size(); i++) {
        ++map[nums1[i]];
    }
    
    for (int j = 0; j < nums2.size(); j++) {
        if (--map[nums2[j]] >= 0) {
            res.push_back(nums2[j]);
        }
    }
    return res;
}
```

### 268. Missing Number
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

#### For example,
Given nums = [0, 1, 3] return 2.

#### Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?
#### 问题
给一个顺序数组，找出遗失的数字
#### 思路
通过^找出遗失的数字，多出的数字一定等于数组长度
#### C++

```
    int missingNumber(vector<int>& nums) {
    
    int result = (int)nums.size();
    int i = 0;
    
    
    for(auto num:nums)
    {
        result ^= num;
        result ^= i;
        i++;
    }
    
    return result;
}
```

### 447. Number of Boomerangs
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

#### Example:
#### Input:
[[0,0],[1,0],[2,0]]

#### Output:
2

#### Explanation:
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
#### 问题
给出一系列坐标，通过这些坐标问你能做几个回旋镖？回旋镖的意思是中点到两点距离相同。
#### 思路
只用unordered_map存储 距离：相同距离的边数，
再使用排列组合将边数的组合数全部加起来
#### C++

```
    int numberOfBoomerangs(vector<pair<int, int>>& points) {
    
    int res = 0;
    
    for (int i = 0; i < points.size(); i++) {
        
        unordered_map<long, int> cnt;
        
        for (int j = 0; j < points.size(); j++) {
            if(i == j) continue;
            int dx = points[i].first - points[j].first;
            int dy = points[i].second - points[j].second;
            
            int key = dx * dx;
            key += dy * dy;
            
            ++cnt[key];
        }
        
        for (auto i : cnt) {
            res += i.second * (i.second - 1);
        }
    }
    return res;

}
```

### 541. Reverse String II
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.
#### Example:

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

#### Restrictions:
The string consists of lower English letters only.
Length of the given string and k will in the range [1, 10000]
#### 问题
给一个数组s，每隔k位翻转字符
#### C++

```
string reverseStr(string s, int k) {

    
    for(int i=0;i<s.size(); i += 2*k){
        reverse(s.begin()+i,min(s.begin()+i+k,s.end()) );
        ;
    }
    
    return s;

}
```

#### 收获
min(s.begin()+i+k,s.end())

### 551. Student Attendance Record I
You are given a string representing an attendance record for a student. The record only contains the following three characters:


```
'A' : Absent.
'L' : Late.
'P' : Present.
A student could be rewarded if his attendance record doesn't contain more than one 'A' (absent) or more than two continuous 'L' (late).

You need to return whether the student could be rewarded according to his attendance record.

Example 1:
Input: "PPALLP"
Output: True
Example 2:
Input: "PPALLL"
Output: False
```

#### 问题
给一组数组，超过1个A或者连续2个L则输出false
#### 思路
使用map进行统计，遇到L则检查前两位是否是L
#### C++

```
bool checkRecord(string s) {
    
    unordered_map<char, int> map;
    
    for (int i = 0; i < s.size(); i++) {
        if (i>1&&s[i]=='L') {
            if (s[i-1] == 'L'&&s[i-2] == 'L') {
                return false;
            }
        }

        if (s[i] == 'A' && ++map[s[i]]>1) {
            return false;
        }
    }
    
    
    return true;
}
```

#### 别人写法

```
bool checkRecord(string s) {
    return !regex_search(s, regex("A.*A|LLL"));
}
```
#### 收获
regex_search(s, regex("A.*A|LLL")) 正则表达式   

### 543. Diameter of Binary Tree
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

#### Example:

```
Given a binary tree 
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```


#### Note: The length of path between two nodes is represented by the number of edges between them.
#### 问题
找出二叉树中任意两点的最长距离
#### 思路
使用DFS遍历左右子树，将两棵树的深度相加
#### C++

```
int maxDepth = 0;

int dfs(TreeNode * root)
{
    if (root ==NULL) {
        return 0;
    }
  
    int leftDepth = dfs(root->left);
    int rightDepth = dfs(root->right);
    
    if (leftDepth + rightDepth > maxDepth) {
        maxDepth = leftDepth + rightDepth;
    }
    
    return max(leftDepth + 1, rightDepth + 1);
}

int diameterOfBinaryTree(TreeNode* root) {
    dfs(root);
    return maxDepth;
}
```


### 108. Convert Sorted Array to Binary Search Tree
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.
#### 问题
将有序数组转化为二叉搜索树
#### 思路
去数组的中位数作为root，递归构建左右子树
#### C++

```
    TreeNode* sortedArrayToBST(vector<int>& nums) {
    
    if (nums.size() == 0) {
        return NULL;
    }
    
    if (nums.size() == 1) {
        return new TreeNode(nums[0]);
    }
    
    int middle = (int)nums.size()/2;
    
    TreeNode *root = new TreeNode(nums[middle]);
    
    vector<int> leftInt(nums.begin(),nums.begin() + middle);
    
    vector<int> rightInt(nums.begin()+middle+1,nums.end());
    
    root->left = sortedArrayToBST(leftInt);
    root->right = sortedArrayToBST(rightInt);
    
    return root;
    
}
```

### 415. Add Strings
Given two non-negative integers num1 and num2 represented as string, return the sum of num1 and num2.

#### Note:


```
The length of both num1 and num2 is < 5100.
Both num1 and num2 contains only digits 0-9.
Both num1 and num2 does not contain any leading zero.
You must not use any built-in BigInteger library or convert the inputs to integer directly.
```

#### 问题
给2个用string表示的数字，将其相加并输出，
#### 思路
进位（carry）累加到下一位计算,将最终结果逆序输出
#### C++

```
string addStrings(string num1, string num2) {
    int carry =0;
    int i = (int)num1.size() - 1;
    int j = (int)num2.size() - 1;
    
    string res = "";
    
    while(i>=0||j>=0||carry)
    {
        int sum = 0;
        if(i>=0) sum += (num1[i] - '0') ; i--;
        if(j>=0) sum += (num2[j] - '0') ; j--;
        
        sum += carry;
        
        carry = sum / 10;
        sum = sum % 10;
        
        res = res + to_string(sum);
    }
    
    reverse(res.begin(), res.end());
    
    return res;
}
```

### 405. Convert a Number to Hexadecimal
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

#### Note:


```
All letters in hexadecimal (a-f) must be in lowercase.
The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
The given number is guaranteed to fit within the range of a 32-bit signed integer.
You must not use any method provided by the library which converts/formats the number to hex directly.
Example 1:

Input:
26

Output:
"1a"
Example 2:

Input:
-1

Output:
"ffffffff"
```

#### 问题
将数转化为16进制数
#### 思路
十进制&15 输出的结果为对应16进制数的最后4位对应的十进制数
#### C++

```
string toHex(int num) {
        int count = 0;
        if(!num) return "0";
        string result;
        while (num && count < 8)
        {
            int temp = num & 15;
            if (temp<10)    result.push_back('0'+ temp);
            else result.push_back('a'+temp-10);
            num = num >> 4;
            count++;
        }
        reverse(result.begin(),result.end());
        return result;
    }
```

#### 收获
num & 15 转化16进制

###
#### 问题
#### 思路
#### C++
#### 收获

###
#### 问题
#### 思路
#### C++
#### 收获
