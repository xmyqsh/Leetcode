#### Suffix Array interview question[转自一亩三分地]
首先定义了suffix string 或者说suffix arrary, 如果有个数组是 ```int[] text = {10, 20, 30, 25}```, 那么   
```
           suffix[0] = {10, 20, 30, 25}
           suffix[1] = {20, 30, 25}
           suffix[2] = {30, 25}
           suffix[3] = {25}
```
如果对这些数组进行lexical order 的排序，我们可以得到 ```suffix[0] < suffix[1] < suffix[3] < suffix[2]``` 问题是:
```     
  input: int[] text. 1point 3acres 
  output: int[] suffix_array_order. 

  e.g.
  input: int[] text = {10, 20, 30, 25}
  output: int[] suffix_array_order = {0, 1, 3, 2}
```

#### [解法一：暴力解法](http://www.geeksforgeeks.org/suffix-array-set-1-introduction/)
* 首先求出所有的prefix
* 然后对做sorting，最快的sorting算法是O(nlogn), 每个长度为n，所以totally 算法复杂度是O(n^2logn)

#### [解法二](http://algorithmsandme.com/2015/01/suffix-array/)

```python

import math

class Suffix:
    def __init__(self):
        self.index = 0
        self.rank = [0, 0]

def customer_cmp(a, b):
    if a.rank[0] == b.rank[0]:
        if a.rank[1] > b.rank[1]:
            return 1
        else: return -1
    elif a.rank[0] > b.rank[0]:
        return 1
    else: return -1

def printp(p):
    for i in xrange(len(p)):
        for j in xrange(len(p[0])):
            print "p[{0}][{1}]:  {2:3d}".format(i, j, p[i][j])
        print "\n"

def printsuffix(suffix, n):
    for i in xrange(n):
        print "i: %d, index: %d rank[0]: %d, rank[1]: %d" \
                %(i, suffix[i].index, suffix[i].rank[0], suffix[i].rank[1])
    print  "\n"
    
def build_suffixarr(s):
    length = len(s)
    steps = int(math.ceil(math.log(length,2)))
    print "steps: ", steps
    suffixes = [Suffix() for i in xrange(length)]
    p = [ [0 for j in xrange(length)] for i in xrange(steps+1)]

    for i in xrange(length):
        p[0][i] = ord(s[i]) - ord('a')
    
    printp(p)

    cnt = 1
    for k in xrange(1,steps+1):
        for i in xrange(length):
            suffixes[i].rank[0] = p[k-1][i]
            if i + cnt < length:
                suffixes[i].rank[1] = p[k - 1][i + cnt]
            else:
                suffixes[i].rank[1] = -1
            suffixes[i].index = i

        printsuffix(suffixes, length)

        suffixes.sort(customer_cmp)

        print "after sort:"
        printsuffix(suffixes, length)

        for i in xrange(length):
            if i > 0 and suffixes[i].rank[0] == suffixes[i - 1].rank[0] and \
                    suffixes[i].rank[1] == suffixes[i - 1].rank[1]:
                        p[k][suffixes[i].index] = p[k][suffixes[i-1].index]
            else:
                p[k][suffixes[i].index] = i

        print "after sort:"
        printp(p)
        cnt <<= 1
    for i in xrange(length):
        print "result: ", p[steps][i]

print build_suffixarr('banana')


# Sample output and debug information

[JINZH2-M-20GQ: ~/Desktop/Python_training/Leetcode]: python suffix_array.py 
steps:  3
p[0][0]:    1
p[0][1]:    0
p[0][2]:   13
p[0][3]:    0
p[0][4]:   13
p[0][5]:    0


p[1][0]:    0
p[1][1]:    0
p[1][2]:    0
p[1][3]:    0
p[1][4]:    0
p[1][5]:    0


p[2][0]:    0
p[2][1]:    0
p[2][2]:    0
p[2][3]:    0
p[2][4]:    0
p[2][5]:    0


p[3][0]:    0
p[3][1]:    0
p[3][2]:    0
p[3][3]:    0
p[3][4]:    0
p[3][5]:    0


i: 0, index: 0 rank[0]: 1, rank[1]: 0
i: 1, index: 1 rank[0]: 0, rank[1]: 13
i: 2, index: 2 rank[0]: 13, rank[1]: 0
i: 3, index: 3 rank[0]: 0, rank[1]: 13
i: 4, index: 4 rank[0]: 13, rank[1]: 0
i: 5, index: 5 rank[0]: 0, rank[1]: -1


after sort:
i: 0, index: 5 rank[0]: 0, rank[1]: -1
i: 1, index: 3 rank[0]: 0, rank[1]: 13
i: 2, index: 1 rank[0]: 0, rank[1]: 13
i: 3, index: 0 rank[0]: 1, rank[1]: 0
i: 4, index: 4 rank[0]: 13, rank[1]: 0
i: 5, index: 2 rank[0]: 13, rank[1]: 0


after sort:
p[0][0]:    1
p[0][1]:    0
p[0][2]:   13
p[0][3]:    0
p[0][4]:   13
p[0][5]:    0


p[1][0]:    3
p[1][1]:    1
p[1][2]:    4
p[1][3]:    1
p[1][4]:    4
p[1][5]:    0


p[2][0]:    0
p[2][1]:    0
p[2][2]:    0
p[2][3]:    0
p[2][4]:    0
p[2][5]:    0


p[3][0]:    0
p[3][1]:    0
p[3][2]:    0
p[3][3]:    0
p[3][4]:    0
p[3][5]:    0


i: 0, index: 0 rank[0]: 3, rank[1]: 4
i: 1, index: 1 rank[0]: 1, rank[1]: 1
i: 2, index: 2 rank[0]: 4, rank[1]: 4
i: 3, index: 3 rank[0]: 1, rank[1]: 0
i: 4, index: 4 rank[0]: 4, rank[1]: -1
i: 5, index: 5 rank[0]: 0, rank[1]: -1


after sort:
i: 0, index: 5 rank[0]: 0, rank[1]: -1
i: 1, index: 3 rank[0]: 1, rank[1]: 0
i: 2, index: 1 rank[0]: 1, rank[1]: 1
i: 3, index: 0 rank[0]: 3, rank[1]: 4
i: 4, index: 4 rank[0]: 4, rank[1]: -1
i: 5, index: 2 rank[0]: 4, rank[1]: 4


after sort:
p[0][0]:    1
p[0][1]:    0
p[0][2]:   13
p[0][3]:    0
p[0][4]:   13
p[0][5]:    0


p[1][0]:    3
p[1][1]:    1
p[1][2]:    4
p[1][3]:    1
p[1][4]:    4
p[1][5]:    0


p[2][0]:    3
p[2][1]:    2
p[2][2]:    5
p[2][3]:    1
p[2][4]:    4
p[2][5]:    0


p[3][0]:    0
p[3][1]:    0
p[3][2]:    0
p[3][3]:    0
p[3][4]:    0
p[3][5]:    0


i: 0, index: 0 rank[0]: 3, rank[1]: 4
i: 1, index: 1 rank[0]: 2, rank[1]: 0
i: 2, index: 2 rank[0]: 5, rank[1]: -1
i: 3, index: 3 rank[0]: 1, rank[1]: -1
i: 4, index: 4 rank[0]: 4, rank[1]: -1
i: 5, index: 5 rank[0]: 0, rank[1]: -1


after sort:
i: 0, index: 5 rank[0]: 0, rank[1]: -1
i: 1, index: 3 rank[0]: 1, rank[1]: -1
i: 2, index: 1 rank[0]: 2, rank[1]: 0
i: 3, index: 0 rank[0]: 3, rank[1]: 4
i: 4, index: 4 rank[0]: 4, rank[1]: -1
i: 5, index: 2 rank[0]: 5, rank[1]: -1


after sort:
p[0][0]:    1
p[0][1]:    0
p[0][2]:   13
p[0][3]:    0
p[0][4]:   13
p[0][5]:    0


p[1][0]:    3
p[1][1]:    1
p[1][2]:    4
p[1][3]:    1
p[1][4]:    4
p[1][5]:    0


p[2][0]:    3
p[2][1]:    2
p[2][2]:    5
p[2][3]:    1
p[2][4]:    4
p[2][5]:    0


p[3][0]:    3
p[3][1]:    2
p[3][2]:    5
p[3][3]:    1
p[3][4]:    4
p[3][5]:    0


result:  3
result:  2
result:  5
result:  1
result:  4
result:  0
None


```






* 第二题： input:  int[] text, int[] subText
*              output: boolean isExist
* 检查text数组中有没有一个subarray 是subText。要求时间小于O(N^2)， N == text.length;
* 这里假设我们有了第一题的 suffix_array_order.
* (做法是binary search)



#### Reference:
* [GeeksForGeeks Tutorial] (http://www.geeksforgeeks.org/suffix-array-set-1-introduction/)
* [Introduction pdf] (http://www.cs.jhu.edu/~langmea/resources/lecture_notes/suffix_arrays.pdf)
