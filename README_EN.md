<!-- /TOC -->

# Python Basics
## File operations
### 1. There is a jsonline format file file.txt with a size of about 10K
```python
def get_lines():
    with open('file.txt','rb') as f:
        return f.readlines()

if __name__ =='__main__':
    for e in get_lines():
        process(e) # Process each row of data
```
Now we have to process a file with a size of 10G, but the memory is only 4G. If only the get_lines function is modified and other codes remain unchanged, how should this be achieved? What are the issues that need to be considered?
```python
def get_lines():
    with open('file.txt','rb') as f:
        for i in f:
            yield i
```
Personally think: It is better to set the number of rows returned each time, otherwise there are too many reads.
```
def get_lines():
    l = []
    with open('file.txt','rb') as f:
      data = f.readlines(60000)
    l.append(data)
    yield l
```
Method provided by Pandaaaa906
```python
from mmap import mmap


def get_lines(fp):
    with open(fp,"r+") as f:
        m = mmap(f.fileno(), 0)
        tmp = 0
        for i, char in enumerate(m):
            if char==b"\n":
                yield m[tmp:i+1].decode()
                tmp = i+1

if __name__=="__main__":
    for i in get_lines("fp_some_huge_file"):
        print(i)
```
The problems to be considered are: the memory is only 4G and cannot read 10G files at one time, and the data needs to be read in batches to record the location of each data read. If the size of the data read in batches is too small, it will take too much time in the read operation.
https://stackoverflow.com/questions/30294146/python-fastest-way-to-process-large-file

### 2. Add missing code
```python
def print_directory_contents(sPath):
"""
This function receives the name of the folder as an input parameter
Returns the path of the file in the folder
And the path to the file in its containing folder
"""
import os
for s_child in os.listdir(s_path):
    s_child_path = os.path.join(s_path, s_child)
    if os.path.isdir(s_child_path):
        print_directory_contents(s_child_path)
    else:
        print(s_child_path)
```
## Modules and packages
### 3. Enter the date, and determine which day is the day of the year?
```python
import datetime
def dayofyear():
    year = input("Please enter the year: ")
    month = input("Please enter the month: ")
    day = input("Please enter the day: ")
    date1 = datetime.date(year=int(year),month=int(month),day=int(day))
    date2 = datetime.date(year=int(year),month=1,day=1)
    return (date1-date2).days+1
```
### 4. Disrupt a sorted list object alist?
```python
import random
alist = [1,2,3,4,5]
random.shuffle(alist)
print(alist)
```
## type of data
### 5. Existing dictionary d = {'a':24,'g':52,'i':12,'k':33} Please sort by value?
```python
sorted(d.items(),key=lambda x:x[1])
```
    x[0] represents sorting by key; x[1] represents sorting by value.
### 6. Dictionary comprehension
```python
d = {key:value for (key,value) in iterable}
```
### 7. Please reverse the string "aStr"?
```python
print("aStr"[::-1])
```
### 8. Process the string "k:1 |k1:2|k2:3|k3:4" into a dictionary {k:1,k1:2,...}
```python
str1 = "k:1|k1:2|k2:3|k3:4"
def str2dict(str1):
    dict1 = {}
    for iterms in str1.split('|'):
        key,value = iterms.split(':')
        dict1[key] = value
    return dict1
#Dictionary derivation
d = {k:int(v) for t in str1.split("|") for k, v in (t.split(":"), )}
```
### 9. Please sort by the age of the elements in alist from largest to smallest
```python
alist = [{'name':'a','age':20},{'name':'b','age':30},{'name':'c','age':25} ]
def sort_by_age(list1):
    return sorted(alist,key=lambda x:x['age'],reverse=True)
```
### 10. What will be the output of the following code?
```python
list = ['a','b','c','d','e']
print(list[10:])
```
The code will output [], no IndexError error will be generated, as expected, try to get a member of a list with an index that exceeds the number of members. For example, trying to get the members of list[10] and later will result in IndexError. However, trying to get a slice of the list, the initial index exceeds the number of members will not generate IndexError, but only return an empty list. This has become a particularly nauseating incurable disease, because there are no errors during operation, making it difficult to track down bugs.
### 11. Write a list production to generate an arithmetic sequence with a tolerance of 11
```python
print([x*11 for x in range(10)])
```
### 12. Given two lists, how to find the same elements and different elements?
```python
list1 = [1,2,3]
list2 = [3,4,5]
set1 = set(list1)
set2 = set(list2)
print(set1 & set2)
print(set1 ^ set2)
```
### 13. Please write a piece of python code to delete duplicate elements in the list?
```python
l1 = ['b','c','d','c','a','a']
l2 = list(set(l1))
print(l2)
```
Use the sort method of the list class:
```python
l1 = ['b','c','d','c','a','a']
l2 = list(set(l1))
l2.sort(key=l1.index)
print(l2)
```
It can also be written like this:
```python
l1 = ['b','c','d','c','a','a']
l2 = sorted(set(l1),key=l1.index)
print(l2)
```
You can also use traversal:
```python
l1 = ['b','c','d','c','a','a']
l2 = []
for i in l1:
    if not i in l2:
        l2.append(i)
print(l2)
```
### 14. Given two lists A, B, please use to find the same and different elements in A and B
```python
Same elements in A and B: print(set(A)&set(B))
Different elements in A, B: print(set(A)^set(B))
```
## Corporate Interview Questions
### 15. What is the difference between the new python class and the classic class?
a. In python, all classes that inherit object are new-style classes

b. There are only new-style classes in Python3

c. In Python2, objects that inherit object are new-style classes, and those that do not have parent classes are classic classes

d. Classic classes are currently not used in Python

e. Maintain the unity of class and type. The results of executing a.__class__ and type(a) on instances of new-style classes are the same, but they are different for old-style classes.

f. The search order for multiple inherited attributes is different. The new-style class uses breadth-first search, and the old-style class uses depth-first search.

### 16. How many built-in data structures in python?
a. Integer type int, long integer type long, floating point type float, complex number complex

b. String str, list list, tuple

c. Dictionary dict, set

d. There is no long in Python3, only int with infinite precision

### 17. How does python implement the singleton mode? Please write two implementation methods?
The first method: use a decorator
```python
def singleton(cls):
    instances = {}
    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return wrapper
    
    
@singleton
class Foo(object):
    pass
foo1 = Foo()
foo2 = Foo()
print(foo1 is foo2) # True
```
The second method: use the base class
New is the method to actually create an instance object, so rewrite the new method of the base class to ensure that only one instance is generated when the object is created
```python
class Singleton(object):
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls,'_instance'):
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance
    
    
class Foo(Singleton):
    pass

foo1 = Foo()
foo2 = Foo()

print(foo1 is foo2) # True
```
The third method: metaclass. Metaclass is a class used to create class objects. When a class object creates an instance object, the call method must be called. Therefore, when calling call, ensure that only one instance is always created. Type is the python meta class
```python
class Singleton(type):
    def __call__(cls, *args, **kwargs):
        if not hasattr(cls,'_instance'):
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instance


# Python2
class Foo(object):
    __metaclass__ = Singleton

# Python3
class Foo(metaclass=Singleton):
    pass

foo1 = Foo()
foo2 = Foo()
print(foo1 is foo2) # True

```

### 18. Reverse an integer, for example -123 --> -321
```python
class Solution(object):
    def reverse(self,x):
        if -10<x<10:
            return x
        str_x = str(x)
        if str_x[0] !="-":
            str_x = str_x[::-1]
            x = int(str_x)
        else:
            str_x = str_x[1:][::-1]
            x = int(str_x)
            x = -x
        return x if -2147483648<x<2147483647 else 0
if __name__ =='__main__':
    s = Solution()
    reverse_int = s.reverse(-120)
    print(reverse_int)
```
### 19. Design and implement to traverse directories and subdirectories, and grab .pyc files
the first method:
```python
import os

def get_files(dir,suffix):
    res = []
    for root, dirs, files in os.walk(dir):
        for filename in files:
            name,suf = os.path.splitext(filename)
            if suf == suffix:
                res.append(os.path.join(root,filename))

    print(res)

get_files("./",'.pyc')
```
The second method:
```python
import os

def pick(obj):
    if obj.endswith(".pyc"):
        print(obj)
    
def scan_path(ph):
    file_list = os.listdir(ph)
    for obj in file_list:
        if os.path.isfile(obj):
            pick(obj)
        elif os.path.isdir(obj):
            scan_path(obj)
    
if __name__=='__main__':
    path = input('input directory')
    scan_path(path)
```
The third method
```python
from glob import iglob


def func(fp, postfix):
    for i in iglob(f"{fp}/**/*{postfix}", recursive=True):
        print(i)

if __name__ == "__main__":
    postfix = ".pyc"
    func("K:\Python_script", postfix)
```
### 20. One line of code to achieve the sum of 1-100
```python
count = sum(range(0,101))
print(count)
```
### 21.Python-The correct way to delete elements when traversing the list
Traverse in the new list operation, delete in the original list operation
```python
a = [1,2,3,4,5,6,7,8]
print(id(a))
print(id(a[:]))
for i in a[:]:
    if i>5:
        pass
    else:
        a.remove(i)
    print(a)
print('-----------')
print(id(a))

```
```python
#filter
a=[1,2,3,4,5,6,7,8]
b = filter(lambda x: x>5,a)
print(list(b))
```
List comprehension
```python
a=[1,2,3,4,5,6,7,8]
b = [i for i in a if i>5]
print(b)
```
Delete in reverse order
Because the list is always ‘forward’, it can be traversed in reverse order. Even if the following elements are modified, the elements that have not been traversed and their coordinates remain unchanged
```python
a=[1,2,3,4,5,6,7,8]
print(id(a))
for i in range(len(a)-1,-1,-1):
    if a[i]>5:
        pass
    else:
        a.remove(a[i])
print(id(a))
print('-----------')
print(a)
```
### 22. String operation topic
Full-letter short sentence PANGRAM is a sentence containing all English letters, such as: A QUICK BROWN FOX JUMPS OVER THE LAZY DOG. Define and implement a method get_missing_letter, pass in a string acceptance number, and the returned parameter string becomes a PANGRAM. Missing characters. The case in the incoming string parameters should be ignored, and the return should be all lowercase characters and sorted alphabetically (please ignore all non-ACSII characters)

**The following example is for explanation, double quotes do not need to be considered:**

(0)Input: "A quick brown for jumps over the lazy dog"

Returns: ""

(1) Input: "A slow yellow fox crawls under the proactive dog"

Returns: "bjkmqz"

(2) Input: "Lions, and tigers, and bears, oh my!"

Returns: "cfjkpquvwxz"

(3) Input: ""

Returns: "abcdefghijklmnopqrstuvwxyz"

```python
def get_missing_letter(a):
    s1 = set("abcdefghijklmnopqrstuvwxyz")
    s2 = set(a.lower())
    ret = "".join(sorted(s1-s2))
    return ret
    
print(get_missing_letter("python"))

# other ways to generate letters
# range("a", "z")
# method one:
import string
letters = string.ascii_lowercase
# Method Two:
letters = "".join(map(chr, range(ord('a'), ord('z') + 1)))
```

### 23. Mutable and Immutable Types
1. Variable types include list and dict. Immutable types include string, number, tuple.

2. When the modification operation is performed, the variable type transfers the address in the memory, that is, directly modifies the value in the memory, and does not open up new memory.

3. When the immutable type is changed, the value in the original memory address is not changed, but a new memory is opened, the value in the original address is copied over, and the value in this newly opened memory is operated.

### 24. What is the difference between is and ==?
is: The comparison is whether the id values ​​of the two objects are equal, that is, whether the two objects are the same instance object. Point to the same memory address

==: Whether the contents/values ​​of the two objects to be compared are equal, the eq() method of the object will be called by default
### 25. Find all odd numbers in the list and construct a new list
```python
a = [1,2,3,4,5,6,7,8,9,10]
res = [i for i in a if i%2==1]
print(res)
```
### 26. Write 1+2+3+10248 with one line of python code
```python
from functools import reduce
#1. Use sum built-in sum function
num = sum([1,2,3,10248])
print(num)
#2.reduce function
num1 = reduce(lambda x,y :x+y,[1,2,3,10248])
print(num1)
```
### 27. What is the scope of variables in Python? (Variable search order)
LEGB order of function scope

1. What is LEGB?

L: the internal scope of the local function

E: Inside the enclosing function and between the embedded function

G: global scope

B: build-in built-in function

Python's search in the function is divided into 4 types, called LEGB, which is exactly the order to search
### 28. The string `"123"` is converted to `123` without using built-in api, such as `int()`
Method 1: Use the `str` function
```python
def atoi(s):
    num = 0
    for v in s:
        for j in range(10):
            if v == str(j):
                num = num * 10 + j
    return num
```
Method 2: Use the `ord` function
```python
def atoi(s):
    num = 0
    for v in s:
        num = num * 10 + ord(v)-ord('0')
    return num
```
Method 3: Use the `eval` function
```python
def atoi(s):
    num = 0
    for v in s:
        t = "%s * 1"% v
        n = eval(t)
        num = num * 10 + n
    return num
```
Method four: Combine method two, use `reduce`, one-line solution
```python
from functools import reduce
def atoi(s):
    return reduce(lambda num, v: num * 10 + ord(v)-ord('0'), s, 0)
```
### 29.Given an array of integers
Given an integer array and a target value, find the two numbers in the array whose sum is the target value. You can assume that each input corresponds to only one answer, and the same elements cannot be reused. Example: Given nums = [2,7,11,15], target=9 because nums[0]+nums[1] = 2+7 =9, so return [0,1]
```python
class Solution:
    def twoSum(self,nums,target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        d = {}
        size = 0
        while size <len(nums):
            if target-nums[size] in d:
                if d[target-nums[size]] <size:
                    return [d[target-nums[size]],size]
                else:
                    d[nums[size]] = size
                size = size +1
solution = Solution()
list = [2,7,11,15]
target = 9
nums = solution.twoSum(list,target)
print(nums)
```

```

class Solution(object):
    def twoSum(self, nums, target):
        for i in range(len(nums)):
            num = target-nums[i]
            if num in nums[i+1:]:
                return [i, nums.index(num,i+1)]

```
Sort the dictionary in the list: Suppose there are the following list objects, alist=[{"name":"a","age":20},{"name":"b","age":30},{" name":"c","age":25}], sort the elements in alist according to age from largest to smallest alist=[{"name":"a","age":20},{"name" :"b","age":30},{"name":"c","age":25}]
```python
alist_sort = sorted(alist,key=lambda e: e.__getitem__('age'),reverse=True)
```

### 30.python code to delete duplicate elements in a list
```python
def distFunc1(a):
    """Use a collection to remove duplicates"""
    a = list(set(a))
    print(a)

def distFunc2(a):
    """Retrieve the data from one list and put it in another list, making judgments in the middle"""
    list = []
    for i in a:
        if i not in list:
            list.append(i)
    #If you need to sort, use sort
    list.sort()
    print(list)

def distFunc3(a):
    """Use Dictionary"""
    b = {}
    b = b.fromkeys(a)
    c = list(b.keys())
    print(c)

if __name__ == "__main__":
    a = [1,2,4,2,4,5,7,10,5,5,7,8,9,0,3]
    distFunc1(a)
    distFunc2(a)
    distFunc3(a)
  
```
### 31. Count the 10 most frequent words in a text?
```python
import re

# method one
def test(filepath):
    
    distone = {}

    with open(filepath) as f:
        for line in f:
            line = re.sub("\W+", "", line)
            lineone = line.split()
            for keyone in lineone:
                if not distone.get(keyone):
                    distone[keyone] = 1
                else:
                    distone[keyone] += 1
    num_ten = sorted(distone.items(), key=lambda x:x[1], reverse=True)[:10]
    num_ten =[x[0] for x in num_ten]
    return num_ten
    
 
# Method Two 
# Use most_common in the built-in Counter
import re
from collections import Counter


def test2(filepath):
    with open(filepath) as f:
        return list(map(lambda c: c[0], Counter(re.sub("\W+", "", f.read()).split()).most_common(10)))
```
### 32. Please write a function that satisfies the following conditions
The input of this function is a list containing only numbers, and a new list is output, each element of which must meet the following conditions:

1. The element is even

2. The element is in an even position in the original list (index is an even number)

```python
def num_list(num):
    return [i for i in num if i %2 ==0 and num.index(i)%2==0]

num = [0,1,2,3,4,5,6,7,8,9,10]
result = num_list(num)
print(result)
```
### 33. Use a single list comprehension to generate a new list
The list only contains values ​​that meet the following conditions, and the elements are even-numbered slices in the original list
```python
list_data = [1,2,5,8,10,3,18,6,20]
res = [x for x in list_data[::2] if x %2 ==0]
print(res)
```
### 34. Generate [1,4,9,16,25,36,49,64,81,100] with one line of code
```python
[x * x for x in range(1,11)]
```
### 35. Enter a certain year, certain month, and certain day, to determine which day is the day of the year?
```python
import datetime

y = int(input("Please enter the year of 4 digits:"))
m = int(input("Please enter the month:"))
d = int(input("Please enter which day is it"))

targetDay = datetime.date(y,m,d)
dayCount = targetDay-datetime.date(targetDay.year -1,12,31)
print("%s is the %s day of %s year."%(targetDay,y,dayCount.days))
```
### 36. Two ordered lists, l1, l2, can not use extend to merge these two lists
```python
def loop_merge_sort(l1,l2):
    tmp = []
    while len(l1)>0 and len(l2)>0:
        if l1[0] <l2[0]:
            tmp.append(l1[0])
            del l1[0]
        else:
            tmp.append(l2[0])
            del l2[0]
    while len(l1)>0:
        tmp.append(l1[0])
        del l1[0]
    while len(l2)>0:
        tmp.append(l2[0])
        del l2[0]
    return tmp
```
### 37. Given an arbitrary length array, implement a function
Let all odd numbers come before even numbers, and sort the odd numbers in ascending order and even numbers in descending order. For example, the string '1982376455' becomes '1355798642'
```python
# method one
def func1(l):
    if isinstance(l, str):
        l = [int(i) for i in l]
    l.sort(reverse=True)
    for i in range(len(l)):
        if l[i]% 2> 0:
            l.insert(0, l.pop(i))
    print(``.join(str(e) for e in l))

# Method Two
def func2(l):
    print("".join(sorted(l, key=lambda x: int(x)% 2 == 0 and 20-int(x) or int(x))))
```
### 38. Write a function to find the second largest number in an integer array
```python
def find_second_large_num(num_list):
    """
    Find the second largest number in the array
    """
    # method one
    # Sort directly, output the second to last number
    tmp_list = sorted(num_list)
    print("Method One\nSecond_large_num is :", tmp_list[-2])
    
    # Method Two
    # Set two flags, one to store the largest number and the other to store the second largest number
    # two stores the next largest value, one stores the maximum value, and traverses the array once. First, judge whether it is greater than one. If it is greater, give the value of one to two, and give the value of num_list[i] to one. Otherwise, compare whether it is greater than two. Greater than directly give the value of num_list[i] to two, otherwise pass
    one = num_list[0]
    two = num_list[0]
    for i in range(1, len(num_list)):
        if num_list[i]> one:
            two = one
            one = num_list[i]
        elif num_list[i]> two:
            two = num_list[i]
    print("Method Two\nSecond_large_num is :", two)
    
    # Method Three
    # Use reduce and logical symbols (and, or)
    # The basic idea is the same as Method 2, but there is no need to use if for judgment.
    from functools import reduce
    num = reduce(lambda ot, x: ot[1] <x and (ot[1], x) or ot[0] <x and (x, ot[1]) or ot, num_list, (0, 0) )[0]
    print("Method Three\nSecond_large_num is :", num)
    
    
if __name__ =='__main___':
    num_list = [34, 11, 23, 56, 78, 0, 9, 12, 3, 7, 5]
    find_second_large_num(num_list)
```
### 39. Read the code and what is their output?
```python
def multi():
    return [lambda x: i*x for i in range(4)]
print([m(3) for m in multi()])
```
The correct answer is [9,9,9,9] instead of [0,3,6,9]. The reason is the late binding of Python’s closure, which means that the variables in the closure are in When the internal function is called, it is looked up, because when the last function is called, the for loop has been completed, and the value of i is 3 at the end, so the i of each return value is 3, so the final result is [9, 9,9,9]
### 40. Count the number of occurrences of characters in a string
```python
# method one
def count_str(str_data):
    """ defines a function of the number of occurrences of a character """
    dict_str = {}
    for i in str_data:
        dict_str[i] = dict_str.get(i, 0) + 1
    return dict_str
dict_str = count_str("AAABBCCAC")
str_count_data = ""
for k, v in dict_str.items():
    str_count_data += k + str(v)
print(str_count_data)

# Method Two
from collections import Counter

print("".join(map(lambda x: x[0] + str(x[1]), Counter("AAABBCCAC").most_common())))
```
### 41. Specific usage and scenarios of super function
https://python3-cookbook.readthedocs.io/zh_CN/latest/c08/p07_calling_method_on_parent_class.html

# Python Advanced
## Metaclass
### 42. What is the difference between class methods, class instance methods, and static methods in Python?
Class method: It is a method of a class object. It needs to be decorated with @classmethod at the top when it is defined. The formal parameter is cls, which means the class object. Both the class object and the instance object can be called.

Class instance method: is the method of class instantiation object, only instance object can be called, the formal parameter is self, which refers to the object itself;

Static method: It is an arbitrary function, decorated with @staticmethod above it, which can be directly called by the object, the static method actually has nothing to do with the class
### 43. Traverse all the attributes of an object and print each attribute name?
```python
class Car:
    def __init__(self,name,loss): # loss [price, fuel consumption, kilometers]
        self.name = name
        self.loss = loss
    
    def getName(self):
        return self.name
    
    def getPrice(self):
        # Get car price
        return self.loss[0]
    
    def getLoss(self):
        # Get car loss value
        return self.loss[1] * self.loss[2]

Bmw = Car("BMW",[60,9,500]) # Instantiate a BMW car object
print(getattr(Bmw,"name")) # Use getattr() to pass in the object name and attribute value.
print(dir(Bmw)) # Get all the attributes and methods of Bmw
```
### 44. Write a class and make it support as many operators as possible?
```python
class Array:
    __list = []
    
    def __init__(self):
        print "constructor"
    
    def __del__(self):
        print "destruct"
    
    def __str__(self):
        return "this self-defined array class"

    def __getitem__(self,key):
        return self.__list[key]
    
    def __len__(self):
        return len(self.__list)

    def Add(self,value):
        self.__list.append(value)
    
    def Remove(self,index):
        del self.__list[index]
    
    def DisplayItems(self):
        print "show all items---"
        for item in self.__list:
            print item
    
        
```
### 45. Introduce what are the disadvantages of Cython, Pypy Cpython Numba
Cython
### 46. Please describe the difference and connection between abstract class and interface class

1. Abstract class: A series of methods are specified, and methods that must be implemented by inherited classes are specified. Because of the existence of abstract methods, abstract classes cannot be instantiated. You can understand the abstract class as the rough house, doors and windows, and the style of the wall is up to you, so the difference between the abstract class and the ordinary class as the base class is that it is more restrictive

2. Interface class: similar to abstract class, the method defined in the interface must be implemented by the reference class, but the fundamental difference between it and the abstract class lies in its purpose: the rules for communicating with different individuals, you need to enter the dormitory The key, this key is the interface between you and the dormitory, and your roommate also has this interface, so he can also enter the dormitory. If you use a mobile phone to talk, then the mobile phone is the interface for you to communicate with others

3. Differences and associations:

1. An interface is a variant of an abstract class. All methods in the interface are abstract, and there can be non-abstract methods in an abstract class. An abstract class is a class that declares the existence of a method without realizing it.

2. Interfaces can be inherited, but abstract classes cannot

3. Interface definition method, there is no implementation code, and abstract class can implement some methods

4. The basic data type in the interface is static but the abstract class is not

### 47. How to dynamically get and set the attributes of an object in Python?

```python
if hasattr(Parent,'x'):
    print(getattr(Parent,'x'))
    setattr(Parent,'x',3)
print(getattr(Parent,'x'))
```



## Memory management and garbage collection mechanism
### 48. What operations will cause Python memory overflow, and how to deal with it?
### 49. Regarding Python memory management, the following statement is wrong B

A, the variable does not need to be declared in advance B, the variable does not need to be created and assigned first and used directly

C, the variable does not need to specify the type D, you can use del to release resources

### 50. Python memory management mechanism and tuning methods?

Memory management mechanism: reference counting, garbage collection, memory pool

Reference counting: Reference counting is a very efficient means of memory management. When a Python object is referenced, its reference count increases by 1,

When it is no longer referenced by a variable, the count is reduced by 1, and when the reference count is equal to 0, the object is deleted. Weak references do not increase the reference count

Garbage collection:

1. Reference counting

Reference counting is also a garbage collection mechanism, and it is also the most intuitive and simple garbage collection technique. When the reference count of an object in Python drops to 0, it means that there is no reference to the object, and the object becomes garbage to be recycled. For example, a newly created object is assigned to a reference, and the reference count of the object becomes 1. If the reference is deleted and the reference count of the object is 0, then the object can be garbage collected. However, if there is a circular reference, the reference counting mechanism no longer plays an effective role.

2. Mark removal

https://foofish.net/python-gc.html

Tuning methods

1. Manual garbage collection

2. Raise the garbage collection threshold

3. Avoid circular references

### 51. What is a memory leak? How to avoid it?

**Memory leak** refers to the program's failure to release memory that is no longer in use due to negligence or error. Memory leak does not mean the physical disappearance of the memory, but after the application allocates a certain segment of memory, due to a design error, the control of the segment of memory is lost before the memory is released, resulting in a waste of memory.

Circular references between objects with `__del__()` function are the main cause of memory leaks. When not using an object, use: del object to delete the reference count of an object can effectively prevent memory leaks.

Use the Python extension module gc to view detailed information about objects that cannot be recycled.

You can get the reference count of the object through sys.getrefcount(obj), and judge whether the memory leaks according to whether the return value is 0

## Function
### 52. What are the common list comprehensions in python?

[Expression for variable in list] or [Expression for variable in list if condition]

### 53. Briefly describe the difference between read, readline and readlines?

read read the entire file

readline read the next line

readlines reads the entire file into an iterator for us to traverse

### 54. What is Hash (hash function)?

**Hash function** (English: Hash function), also known as **hash algorithm**, **hash function**, is a method of creating a small digital "fingerprint" from any kind of data. The hash function compresses the message or data into a digest, so that the amount of data becomes smaller, and the format of the data is fixed. This function scrambles and mixes the data and recreates a fingerprint called **hash values** (hash values, hash codes, hash sums, or hashes). The hash value is usually represented by a short string of random letters and numbers

### 55. Python function overload mechanism?

Function overloading is mainly to solve two problems.
1. Variable parameter type.
2. The number of variable parameters.

In addition, a basic design principle is that only when the functions of the two functions are exactly the same except for the different parameter types and number of parameters, then function overloading is used. If the functions of the two functions are actually different, then the functions of the two functions are not the same. Overloading should be used, but a function with a different name should be used.

Okay, so for case 1, the function has the same function, but the parameter type is different, how does python handle it? The answer is that there is no need to deal with it at all, because Python can accept any type of parameter. If the function of the function is the same, then the different parameter types are likely to be the same code in Python, and there is no need to make two different functions.

So for case 2, the function has the same function, but the number of parameters is different, how does python handle it? As you all know, the answer is the default parameters. The problem can be solved by setting the missing parameters as default parameters. Because you assume that the functions are the same, the missing parameters will eventually be needed.

Well, given that both cases 1 and 2 have solutions, python naturally does not need function overloading.

### 56. Write a function to find the second largest number in an integer array
### 57. Handwriting a decorator to judge the time
```python
import datetime


class TimeException(Exception):
    def __init__(self, exception_info):
        super().__init__()
        self.info = exception_info

    def __str__(self):
        return self.info


def timecheck(func):
    def wrapper(*args, **kwargs):
        if datetime.datetime.now().year == 2019:
            func(*args, **kwargs)
        else:
            raise TimeException("Function is outdated")

    return wrapper


@timecheck
def test(name):
    print("Hello {}, 2019 Happy".format(name))


if __name__ == "__main__":
    test("backbp")
```
### 58. Use Python's built-in filter() method to filter?
```python
list(filter(lambda x: x% 2 == 0, range(10)))
```
### 59. 4 principles for writing functions

1. The function design should be as short as possible

2. The function declaration should be reasonable, simple and easy to use

3. The function parameter design should consider backward compatibility

4. A function does only one thing, try to ensure the consistency of the function statement granularity

### 60. Is the method of passing function call parameters by value or by reference?

Python's parameter transfer includes: positional parameters, default parameters, variable parameters, and keyword parameters.

Whether the value of a function is passed by value or by reference depends on the situation:

Immutable parameters are passed by value: Immutable objects like integers and strings are passed by copy, because you can't change the immutable object in place anyway.

Variable parameters are passed by reference: For example, objects such as lists and dictionaries are passed by reference, which is very similar to passing arrays by pointers in the C language. Variable objects can be changed inside functions.

### 61. How to set a global variable in function

```python
globals() # Return a dictionary containing the current remaining global variables.
global variables set to use global variables
```

### 62. Understanding of the default parameters?

Default parameters refer to the default parameters that are called when no parameters are passed in when the function is called. When assigning values ​​while calling the function, the passed parameters will replace the default parameters.

*args is an indefinite length parameter, it can indicate that the input parameter is uncertain and can be any number.

**kwargs are keyword parameters, when assigning values ​​are in the form of key-value pairs, the parameters can be any number of pairs when defining the function

When you are not sure how many parameters will be passed in, you can use two parameters

### 63. How does Mysql restrict IP access?



### 64. Decorator with parameters?

Decorator with fixed length parameters

```python
def new_func(func):
    def wrappedfun(username, passwd):
        if username =='root' and passwd == '123456789':
            print('Certified')
            print('Start to perform additional functions')
            return func()
       else:
            print('User name or password is wrong')
            return
    return wrappedfun

@new_func
def origin():
    print('Start executing function')
origin('root','123456789')
```

Decorator with variable length parameters

```python
def new_func(func):
    def wrappedfun(*parts):
        if parts:
            counts = len(parts)
            print('This system contains', end='')
            for part in parts:
                print(part, '',end='')
            print('wait', counts,'partial')
            return func()
        else:
            print('User name or password is wrong')
            return func()
   return wrappedfun

```

### 65. Why can function names be used as parameters?

Everything in Python is an object, and the function name is the space of the function in memory, and it is also an object

### 66. What is the function of the pass statement in Python?

When writing the code, only the framework ideas are written, and the pass can be used to occupy the position before the specific implementation is written. The program will not report errors and will not perform any operations.

### 67. With such a piece of code, what will print c output and why?

```python
a = 10
b = 20
c = [a]
a = 15
```

Answer: 10 is the corresponding value for strings and numbers.



### 68. Exchange the values ​​of two variables?

```python
a, b = b, a
```



### 69. Map function and reduce function?

```python
map(lambda x: x * x, [1, 2, 3, 4]) # use lambda
# [1, 4, 9, 16]
reduce(lambda x, y: x * y, [1, 2, 3, 4]) # equivalent to ((1 * 2) * 3) * 4
# twenty four
```



### 70. Callback function, how to communicate?

The callback function is to pass the pointer (address) of the function as a parameter to another function, treat the whole function as an object, and assign the value to the called function.

### 71. What are the main built-in data types of Python? The output of print dir( ‘a ’)?

Built-in types: Boolean types, numbers, strings, lists, tuples, dictionaries, sets

Built-in method to output the string'a'

### 72.map(lambda x:xx, [y for y in range(3)]) output?

```
[0, 1, 4]
```

### 73.hasattr() getattr() setattr() How to use the function?

hasattr(object,name) function:

Judge whether there is a name attribute or a name method in an object, return bool value, return True if there is a name attribute (method), otherwise return False.

```python
class function_demo(object):
    name ='demo'
    def run(self):
        return "hello function"
functiondemo = function_demo()
res = hasattr(functiondemo, "name") # Determine whether the object has a name attribute, True
res = hasattr(functiondemo, "run") # Determine whether the object has a run method, True
res = hasattr(functiondemo, "age") # Determine whether the object has age attribute, False
print(res)
```

getattr(object, name[,default]) function:

Get the properties or methods of the object, if it exists, print it out, if it does not exist, print the default value, the default value is optional. Note: If the method returned is an object, the print result is: the memory address of the method. If you need to run this method, you can add parentheses ().

```python
functiondemo = function_demo()
getattr(functiondemo, "name")# Get the name attribute, print it out if it exists --- demo
getattr(functiondemo, "run") # Get the run method, there is the memory address of the print method
getattr(functiondemo, "age") # Get non-existent attributes, report an error
getattr(functiondemo, "age", 18)# Get a non-existent attribute and return a default value
```

setattr(object, name, values) function:

Assign values ​​to the properties of the object. If the properties do not exist, create them first and then assign

```python
class function_demo(object):
    name = "demo"
    def run(self):
        return "hello function"
functiondemo = function_demo()
res = hasattr(functiondemo, "age") # Determine whether the age attribute exists, False
print(res)
setattr(functiondemo, "age", 18) # Assign a value to the age attribute, no return value
res1 = hasattr(functiondemo, "age") # Determine whether the attribute exists again, True
```

Comprehensive use

```python
class function_demo(object):
    name = "demo"
    def run(self):
        return "hello function"
functiondemo = function_demo()
res = hasattr(functiondemo, "addr") # first judge whether it exists
if res:
    addr = getattr(functiondemo, "addr")
    print(addr)
else:
    addr = getattr(functiondemo, "addr", setattr(functiondemo, "addr", "Beijing Capital"))
    print(addr)
```



### 74. Solve the factorial function in one sentence?

```
reduce(lambda x,y: x*y,range(1,n+1))
```



### 75. What is a lambda function? what is the benefit?

A lambda function is a function that can receive any number of parameters (including optional parameters) and return a single expression value

1. The lambda function is relatively light, even if it is used, it is very suitable for a function that needs to be completed, but this function is only used in this place, and even the name is very random

2. Anonymous functions, generally used to provide functional programming services such as filter and map

3. As a callback function, passed to some applications, such as message processing

### 76. What are the conditions for stopping the recursive function?

The termination condition of recursion is generally defined in the recursive function. Before the recursive call, a conditional judgment must be made. According to the result of the judgment, choose whether to continue to call itself or return, and return to terminate the recursion.

Termination condition: determine whether the number of recursion reaches a certain limit value

2. Determine whether the result of the calculation reaches a certain range, etc., and choose according to the design purpose

### 77. What will be the output of the following code? please explain.

```python
def multipliers():
    return [lambda x: i *x for i in range(4)]
print([m(2) for m in multipliers()])

```

The output of the above code is [6,6,6,6], which is not what we thought [0,2,4,6]

How do you modify the definition of multipliers above to produce the desired result?

The cause of the above problem is the delayed binding of python closures. This means that when the internal function is called, the value of the parameter is looked up in the closure. Therefore, when any function returned by multipliers() is called, the value of i will be looked up in the nearby range. At that time, regardless of whether the returned function is called or not, the for loop has been completed and i is assigned the final value of 3.

```python
def multipliers():
    for i in range(4):
        yield lambda x: i *x
```

```python
def multipliers():
    return [lambda x,i = i: i*x for i in range(4)]

```





### 78. What is a lambda function? What are its benefits? Write an anonymous function to find the sum of two numbers

The lambda function is an anonymous function. The lambda function can be used to create a small anonymous function. This function is named after omitting the standard step of declaring a function with def


##  Design Patterns
### 79. Understanding of design patterns, briefly describe the design patterns you know?
Design patterns are summarized, optimized, and reusable solutions to some programming problems we often encounter. A design pattern is not like a class or a library that can directly affect our code. On the contrary, a design pattern is more advanced. It is a method template that must be implemented in a specific situation.
The common ones are factory mode and singleton mode

### 80. Please write a single case by hand
```python
#python2
class A(object):
    __instance = None
    def __new__(cls,*args,**kwargs):
        if cls.__instance is None:
            cls.__instance = objecet.__new__(cls)
            return cls.__instance
        else:
            return cls.__instance
```
### 81. What are the application scenarios of singleton mode?
The application scenario of the singleton mode is generally found under the following conditions:
In the case of resource sharing, avoid performance or loss caused by resource operation, such as log files and application configuration.
In the case of controlling resources, it is convenient to communicate between resources. Such as thread pool, 1, website counter 2, application configuration 3. Multi-thread pool 4 database configuration database connection pool 5. application log application...
### 82. Generate [1,4,9,16,25,36,49,64,81,100] with one line of code
```python
print([x*x for x in range(1, 11)])
```
### 83. Understanding of decorators, and write a decorator that records method execution performance by timer?
The decorator is essentially a callable object, which allows other functions to add additional functions without any code changes. The return value of the decorator is also a function object.

```python
import time
from functools import wraps

def timeit(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.clock()
        ret = func(*args, **kwargs)
        end = time.clock()
        print('used:',end-start)
        return ret
    
    return wrapper
@timeit
def foo():
    print('in foo()'foo())
```
### 84. Explain what is a closure?
Define a function inside the function, and this function uses the variables of the outer function, then this function and some of the variables used are called closures.

### 85. What is the function of the function decorator?
The decorator is essentially a callable object, which can add additional functions to other functions without any code changes. The return value of the decorator is also an object of a function, which is often used in scenarios with aspect requirements. For example: insert log, performance test, transaction processing, cache. In scenarios such as permission verification, with decorators, a large number of similar codes that have nothing to do with the function itself can be extracted concurrently and continue to be used.
Detailed reference: https://manjusaka.itscoder.com/2018/02/23/something-about-decorator/

### 86. The difference between generator and iterator?
Iterators are objects that follow the iteration protocol. Users can use iter() to get an iterator from any sequence (such as list, tuple, dictionary, set, etc.). Another method is to create another form of iterator-generator. To get the next element, use the member function next() (Python 2) or the function next() function (Python 3). When there are no elements, the StopIteration exception is raised. To implement your own iterator, you only need to implement next() (Python 2) or `__next__`() (Python 3)

Generator (Generator), just use the yield statement when you need to return data. Every time next() is called, the generator will return the position it left from (it remembers the position of the last execution of the statement and all data values)


Difference: The generator can do everything that an iterator can do, and because the iter() and next() methods are automatically created, the generator is particularly concise, and the generator is also efficient. You can use generator expressions instead of list comprehensions. At the same time save memory. In addition to the automatic method of creating and saving the state of the program, when the generator terminates, a StopIteration exception is automatically thrown.

Official introduction: https://docs.python.org/3/tutorial/classes.html#iterators
### 87. What type of X?
    X = (i for i in range(10))
    X is generator type
### 88. Please use a line of code to group the list of 1-N integers by 3
```python
N = 100
print ([[x for x in range(1,100)] [i:i+3] for i in range(0,100,3)])
```
### 89. The usage of yield in Python?
Yield is to save the current program execution state. When you use a for loop, it will be calculated every time an element is taken. The function that uses yield is called generator. Like iterator, it has the advantage of not calculating all elements at once, but calculating once, which can save a lot of space. Each calculation of generator requires the previous calculation result, so use yield, otherwise a return , The last calculation result is gone
## Object Oriented
### 90. Mutable and immutable objects in Python?

Immutable object, the value in the memory pointed to by the object cannot be changed. When a variable is changed, since the value it refers to cannot be changed, it is equivalent to copying the original value and then changing it. This will open up a new address, and the variable will point to this new address.

Mutable object, the value in the memory pointed to by the object can be changed. After the variable (referred to as a reference to be precise) is changed, in fact the value it refers to directly changes, no copying occurs, and no new address is opened up. In layman's terms, it is changed in situ.

In Pyhton, numeric types (int and float), string str, and tuple are all immutable types. And the list list, dictionary dict, set set are variable types

### 91. The magic method of Python

Magic methods are special methods that can add magic to your class. If your object implements (overloads) one of these methods, then this method will be called by Python under special circumstances. You can define yourself The desired behavior, and all of this happens automatically. They are often named after two underscores (such as `__init___`,`__len__`). Python's magic method is very powerful, so understanding its usage also changes Particularly important!

The `__init__` constructor is a method that is initialized when an instance is created, but it is not the first method called by instantiation.

`__new__` is the first method called by the instantiated object. It only takes the cls parameters and passes other parameters to `__init___`.

`___new__` is rarely used, but there are scenarios where it is suitable, especially when the class inherits from a type that does not change frequently, such as a primitive ancestor or a string.

`__call__` makes an instance of a class be called like a function

`__getitem__` defines the behavior of obtaining the specified element in the container, which is equivalent to self[key]

`__getattr__` defines the behavior when the user tries to access a non-existent attribute.

`__setattr__` defines the behavior when an attribute is set

`__getattribute___` defines the behavior when an attribute is accessed

### 92. How to implement read-only properties in object-oriented?

Privatize the object and provide an interface for reading data through a shared method

```python
class person:
    def __init__(self, x):
        self.__age = 10
    def age(self):
        return self.__age
t = person(22)
# t.__age =100
print(t.age())
```

The best way

```python
class MyCls(object):
    __weight = 50
    
    @property
    def weight(self):
        return self.__weight
   
```

### 93. Talk about your understanding of object-oriented?

Object-oriented is equivalent to process-oriented. Process-oriented language is a programming method based on functional analysis and algorithm-centric, while object-oriented is a programming idea based on structural analysis and data-centric. . There is a very important thing in object-oriented languages, called classes. Object-oriented has three characteristics: encapsulation, inheritance, and polymorphism.

## Regular expression
### 94. Please write a piece of code to match the ip with a regular pattern?

### 95.a = "abbbccc", use regular matching as abccc, no matter how many b, it will appear once?
    Idea: No matter how many b is replaced with one

    re.sub(r'b+','b', a)
### 96. Python string search and replacement?
    a, str.find(): Positive sequence string search function
    Function prototype:
    str.find(substr [,pos_start [,pos_end]])
    Returns the label of the first letter of the first occurrence of substr in str, or -1 if there is no substr in str, that is, the label of the first letter of the first occurrence of substr from the left.

    Parameter Description:
    str: represents the original string
    substr: represents the string to be searched
    pos_start: represents the starting position of the search, the default is to start the search from the subscript 0
    pos_end: represents the end position of the search

    example:
    'aabbcc.find('bb')' # 2

    b, str.index(): Positive sequence string search function
    The index() function is similar to the find() function. In Python, the position of the first occurrence of the substring is also found in the string. Unlike find(), an exception is thrown if it is not found.

    Function prototype:
    str.index(substr [, pos_start, [pos_end]])

    Parameter Description:
    str: represents the original string
    substr: represents the string to be searched
    pos_start: represents the starting position of the search, the default is to start the search from the subscript 0
    pos_end: represents the end position of the search

    example:
    'acdd l1 23'.index('') # 4

    c, str.rfind(): reverse string search function

    Function prototype:
    str.rfind( substr [, pos_start [,pos_ end] ])
    Returns the label of the first letter of the last occurrence of substr in str, or -1 if there is no substr in str, that is, the label of the first letter of the first occurrence of substr from the right.

    Parameter Description:
    str: represents the original string
    substr: represents the string to be searched
    pos_start: represents the starting position of the search, the default is to start the search from the subscript 0
    pos_end: represents the end position of the search

    example:
    'adsfddf'.rfind('d') # 5

    d, str.rindex(): reverse string search function
    The rindex() function is similar to the rfind() function. In Python, the position of the last occurrence of the substring is searched in reverse order in the string. Unlike rfind(), an exception is thrown if it is not found.

    Function prototype:
    str.rindex(substr [, pos_start, [pos_end]])

    Parameter Description:
    str: represents the original string
    substr: represents the string to be searched
    pos_start: represents the starting position of the search, the default is to start the search from the subscript 0
    pos_end: represents the end position of the search

    example:
     'adsfddf'.rindex('d') # 5

    e. Use the re module to find and replace:
Function | Description
---|---
re.match(pat, s) | Match only from the beginning of the string s, such as ('123', '12345') matches, and ('123','01234') is no match, no match Return None on match, return matchobject on match
re.search(pat, s) | Match from any position of the string s, such as ('123','01234') is a match, as long as s can only have a continuous string that matches pat, it will match , If there is no match, return None, if match, return matchobject
re.sub(pat,newpat,s) | re.sub(pat,newpat,s) replaces all consecutive strings that conform to pat contained in s in the string, if newpat is str, then it is replaced with newpat, If newpat is a function, then replace it according to the return value of the function. The two parameters of the sub function with default values ​​are count, which means that only the first few matching strings will be processed at most, and the default is 0 for all processing; the last one is flags, which is 0 by default.

    f. Use replace() to replace:
    Basic usage: object.replace(rgExp,replaceText,max)

    Among them, rgExp and replaceText are required, and max is an optional parameter, which can be omitted.
    rgExp refers to a regular expression pattern or a regular expression object with available flags, and it can also be a String object or text;
    replaceText is a String object or string text;
    max is a number.
    For an object, each rgExp in the object is replaced with replaceText, max times from left to right.

    s1='hello world'
    s1.replace('world','liming')

### 97. When matching HTML tags with Python, what is the difference between <.*> and <.*?>
    The first represents greedy matching, and the second represents non-greedy;
    ? The grammar in the general regular expression means "zero or one match of the left character or expression" is equivalent to {0,1}
    When the? Suffix is ​​after *,+,?,{n},{n,},{n,m}, it represents a non-greedy matching mode, that is to say, match the characters or expressions on the left as little as possible, Here are as few matches as possible. (any character)

    So: The first way of writing is to match as much as possible, that is, the matched string is as long as possible, and the second way of writing is to match as few as possible, that is, the matched string is as short as possible.
    For example, <tag>tag>tag>end, the first will match <tag>tag>tag>, and the second will match <tag>.
### 98. What is the difference between regular expression greedy and non-greedy mode?
    Greedy mode:
    Definition: When using regular expressions to match, it will try to match as many content as possible
    Identifier: +,?, *, {n}, {n,}, {n,m}
    When matching, if the above identifier is encountered, it means that it is a greedy match, and it will match as much content as possible

    Non-greedy mode:
    Definition: When the regular expression is matched, it will match the content that meets the conditions as little as possible. That is, once the match is found to meet the requirements, the match will be successful immediately, and the match will not continue (unless there is g, open the next set of matching)
    Identifier: +?,??, *?, {n}?, {n,}?, {n,m}?
    As you can see, the identifier of non-greedy mode is very regular, that is, the identifier of greedy mode is followed by a?

    Reference article: https://dailc.github.io/2017/07/06/regularExpressionGreedyAndLazy.html

### 99. Write a regular expression that matches letters and underscores at the beginning and numbers at the end?
    s1='_aai0efe00'
    res=re.findall('^[a-zA-Z_]?[a-zA-Z0-9_]{1,}\d$',s1)
    print(res)

### 100. Regular expression operations
### 101. Please match the json string in variable A.
### 102. How to filter expressions in comments?
    Idea: It is mainly to match the range of the emoticon package, and replace the range of the emoticon package with empty
```
import re
pattern = re.compile(u'[\uD800-\uDBFF][\uDC00-\uDFFF]')
pattern.sub('',text)

```
### 103. Briefly describe the difference between search and match in Python
    The match() function only detects whether the beginning of the string matches, and returns the result if the match is successful, otherwise it returns None;
    The search() function will search for a pattern match in the entire string, until the first match is found, and then return an object containing the matching information. The object can get the matched string by calling the group() method. If the string does not match , It returns None.

### 104. Please write a Python regular expression that matches ip
### 105. What is the difference between match and search in Python?
    See question 103

## System Programming
### 106. Process summary
Process: An instance of a program running on an operating system is called a process. The process needs corresponding system resources: memory, time slice, pid.
Create process:
First, import the Process in multiprocessing:
Create a Process object;
When creating a Process object, you can pass parameters;
```python
p = Process(target=XXX,args=(tuple,),kwargs={key:value})
target = the task function specified by XXX, no need to add (),
args=(tuple,)kwargs=(key:value) parameters passed to the task function
```
Use start() to start the process
end process
Pass parameter Demo to the specified function of the child process
```python
import os
from mulitprocessing import Process
import time

def pro_func(name,age,**kwargs):
    for i in range(5):
        print("The child process is running, name=%s,age=%d,pid=%d"%(name,age,os.getpid()))
        print(kwargs)
        time.sleep(0.2)
if __name__ == "__main__":
    #Create Process Object
    p = Process(target=pro_func,args=('小明',18),kwargs={'m':20})
    #Start process
    p.start()
    time.sleep(1)
    #1 second later, immediately end the child process
    p.terminate()
    p.join()
```
Note: global variables are not shared between processes

Communication between processes-Queue

When initializing the Queue() object (for example, q=Queue(), if the maximum acceptable number of messages is not specified in the parentheses, and the obtained number is negative, it means that the number of acceptable messages has no upper limit until the end of the memory)

Queue.qsize(): returns the number of messages contained in the current queue

Queue.empty(): If the queue is empty, return True, otherwise False

Queue.full(): If the queue is full, return True, otherwise False

Queue.get([block[,timeout]]): Get a message in the queue, and then remove it from the queue,

The default value of block is True.

If the block uses the default value and no timeout (in seconds) is set, if the message queue is empty, the program will be blocked (stopped in the state of reading) until the message queue has read the message. If the timeout is set, it will wait timeout seconds, if no message has been read yet, the "Queue.Empty" exception will be thrown:

Queue.get_nowait() is equivalent to Queue.get(False)

Queue.put(item,[block[,timeout]]): write the item message to the queue, the default value of block is True;
If the block uses the default value and the timeout (in seconds) is not set, if the message queue has no space to write, the program will be blocked (stopped in the writing state) until space is free from the message queue. If set If timeout is reached, it will wait for timeout seconds, if there is still no space, it will throw "Queue.Full" exception
If the block value is False, if the message queue has no space to write, it will immediately throw a "Queue.Full" exception;
Queue.put_nowait(item): equivalent to Queue.put(item, False)

Demo of inter-process communication:
```python
from multiprocessing import Process.Queue
import os,time,random
#Write the code executed by the data process:
def write(q):
    for value in ['A','B','C']:
        print("Put %s to queue...",%value)
        q.put(value)
        time.sleep(random.random())
#Read the code executed by the data process
def read(q):
    while True:
        if not q.empty():
            value = q.get(True)
            print("Get %s from queue.",%value)
            time.sleep(random.random())
        else:
            break
if __name__=='__main__':
    #The parent process creates a Queue and passes it to each child process
    q = Queue()
    pw = Process(target=write,args=(q,))
    pr = Process(target=read,args=(q,))
    #Start the child process pw, write:
    pw.start()
    #Wait for pw to end
    pw.join()
    #Start the child process pr, read:
    pr.start()
    pr.join()
    #pr There is an endless loop in the process, you cannot wait for its end, you can only terminate it forcefully:
    print('')
    print('All data are written and read')
```
    Process Pool Pool
```python
#coding:utf-8
from multiprocessing import Pool
import os,time,random

def worker(msg):
    t_start = time.time()
    print("%s starts to execute, the process number is %d"%(msg,os.getpid()))
    # random.random() Randomly generate floating-point numbers between 0-1
    time.sleep(random.random()*2)
    t_stop = time.time()
    print(msg,"Execution completed, time-consuming %0.2f"%(t_stop-t_start))

po = Pool(3)#Define a process pool, the maximum number of processes is 3
for i in range(0,10):
    po.apply_async(worker,(i,))
print("---start----")
po.close()
po.join()
print("----end----")
```
Use Queue in the process pool

If you want to use Pool to create a process, you need to use Queue() in multiprocessing.Manager() instead of multiprocessing.Queue(), otherwise you will get the following error message:

RuntimeError: Queue objects should only be shared between processs through inheritance
```python
from multiprocessing import Manager,Pool
import os,time,random
def reader(q):
    print("reader start (%s), parent process is (%s)"%(os.getpid(),os.getpid()))
    for i in range(q.qsize()):
        print("reader gets the message from Queue:%s"%q.get(True))

def writer(q):
    print("writer started (%s), parent process is (%s)"%(os.getpid(),os.getpid()))
    for i ini "itcast":
        q.put(i)
if __name__ == "__main__":
    print("(%s)start"%os.getpid())
    q = Manager().Queue()#Use Queue in Manager
    po = Pool()
    po.apply_async(wrtier,(q,))
    time.sleep(1)
    po.apply_async(reader,(q,))
    po.close()
    po.join()
    print("(%s)End"%os.getpid())
```
### 107. Talk about your understanding of multi-processes, multi-threads, and coroutines. Does the project use it?
The concept of this question being asked is quite big,
Process: A running program (code) is a process, and the code that is not running is called a program. The process is the smallest unit of system resource allocation. The process has its own independent memory space. All the data is not shared between processes, and the overhead is high.

Thread: The smallest unit of CPU scheduling execution, also called execution path, cannot exist independently, depends on the existence of the process, a process has at least one thread, called the main thread, and multiple threads share memory (data sharing, shared global variables), thus extremely The operation efficiency of the program is greatly improved.

Coroutine: It is a lightweight thread in user mode, and the scheduling of the coroutine is completely controlled by the user. The coroutine has its own register context and stack. When the coroutine is scheduled, save the register context and stack to other places. When switching back, restore the previously saved register context and stack. Directly operating the stack will basically have no kernel switching overhead, and you can access global variables without locking. , So the context switching is very fast.

### 108. What are the asynchronous usage scenarios of Python?
Asynchronous usage scenarios:

1. No shared resources are involved, and shared resources are read-only, that is, non-mutually exclusive operations

2. There is no strict relationship in timing

3. No atomic operation is required, or atomicity can be controlled by other means

4. It is often used for time-consuming operations such as IO operations, because it affects customer experience and performance

5. Does not affect the logic of the main thread

### 109. Multi-threads work together to synchronize the same data mutex?
```python
import threading
import time
class MyThread(threading.Thread):
    def run(self):
        global num
        time.sleep(1)
    
        if mutex.acquire(1):
            num +=1
            msg = self.name +'set num to '+str(num)
            print msg
            mutex.release()
num = 0
mutex = threading.Lock()
def test():
    for i in range(5):
        t = MyThread()
        t.start()
if __name__=="__main__":
    test()
```
### 110. What is multi-threaded competition?
Threads are not independent. Threads in the same process share data. When each thread accesses data resources, there will be a state of competition, that is: data is almost synchronized and will be occupied by multiple threads, causing data confusion, which is the so-called thread insecurity.

So how to solve the multi-threaded competition problem? ---lock

The benefits of locks: Ensure that a certain piece of critical code (shared data resources) can only be executed completely by one thread from beginning to end, which can solve the problem of atomic operations under multi-threaded resource competition.

Disadvantages of locks: Prevents concurrent execution of multiple threads. In fact, a certain piece of code containing locks can only be executed in single-threaded mode, and the efficiency is greatly reduced.

The fatal problem of locks: deadlocks
### 111. Please tell me about thread synchronization in Python?
 One, setDaemon(False)
When a process is started, a main thread will be generated by default, because the thread is the smallest unit of program execution. When multi-threading is set, the main thread will create multiple child threads. In Python, the default is setDaemon(False), the main After the thread finishes its task, it exits. At this time, the child thread will continue to perform its task until the end of its task.

example
```python
import threading
import time

def thread():
    time.sleep(2)
    print('---End of child thread---')

def main():
    t1 = threading.Thread(target=thread)
    t1.start()
    print('---Main thread--End')

if __name__ =='__main__':
    main()
#Results of the
---Main thread--End
---End of child thread---
```
Two, setDaemon (True)
When we use setDaemon(True), this is the child thread as a daemon thread. Once the main thread is executed, all child threads are forcibly terminated

example
```python
import threading
import time
def thread():
    time.sleep(2)
    print(’---End of child thread---')
def main():
    t1 = threading.Thread(target=thread)
    t1.setDaemon(True)#Set the child thread to guard the main thread
    t1.start()
    print('---End of main thread---')

if __name__ =='__main__':
    main()
#Results of the
---The main thread ends--- #Only the main thread ends, and the child threads are forced to end before execution
```
Three, join (thread synchronization)
The work done by join is thread synchronization, that is, after the task of the main thread ends, it enters a blocked state, and waits for the end of all child threads before the main thread terminates.

When setting the daemon thread, the meaning is that the main thread will kill the child thread for the timeout timeout of the child thread, and finally exit the program, so if there are 10 child threads, the total waiting time is the cumulative sum of each timeout, Simply put, it is to give each child thread a timeou time and let him execute it. When the time is up, no matter whether the task is completed or not, it will be killed directly.

When the daemon thread is not set, the main thread will wait for the accumulation of timeout and such a period of time. Once the time is up, the main thread ends, but the child threads are not killed, and the child threads can continue to execute until the child threads are all finished. drop out.

example
```python
import threading
import time

def thread():
    time.sleep(2)
    print('---End of child thread---')

def main():
    t1 = threading.Thread(target=thread)
    t1.setDaemon(True)
    t1.start()
    t1.join(timeout=1)#1 Thread synchronization, the main thread is blocked for 1s, then the main thread ends, and the child threads continue to execute
                        #2 If you don't set the timeout parameter, wait until the child thread ends and the main thread ends
                        #3 If setDaemon=True and timeout=1 are set, the main thread will forcibly kill the child thread after waiting for 1s, and then the main thread ends
    print('---End of main thread---')

if __name__=='__main___':
    main()
```
### 112. Explain what is a lock, and what kinds of locks are there?
Lock (Lock) is an object for thread control provided by python. There are mutex locks, reentrant locks, and deadlocks.

### 113. What is a deadlock?
When several sub-threads compete for system resources, they are all waiting for the other party to release some resources. As a result, no one wants to unlock first, waiting for each other, and the program cannot be executed. This is a deadlock.

GIL lock global interpreter lock

Function: Limit the simultaneous execution of multiple threads to ensure that only one thread executes at the same time, so multithreading in cython is actually pseudo multithreading!

So Python often uses coroutine technology to replace multithreading, and coroutine is a more lightweight thread.

The process and thread switching is determined by the system, and the coroutine is determined by our programmers, and the switch under the module gevent is switched only when it encounters a time-consuming operation.

The relationship between the three: there are threads in the process, and there are coroutines in the threads.
### 114. Multi-threaded interactive access to data, if it is accessed, it will not be accessed?
How to avoid rereading?

Create a visited data list to store the data that has been visited, and add a mutex lock. When multithreading accesses the data, first check whether the data is in the visited list, and skip it if it already exists.

### 115. What is thread safety and what is a mutex?
Each object corresponds to a tag that can be called a "mutual exclusion lock". This tag is used to ensure that at any one time, only one thread can access the object.

System resources are shared among multiple threads in the same process. Multiple threads operate on an object at the same time. One thread has not yet finished the operation, and another thread has already operated on it, resulting in an error in the final result. The operation object adds a mutex lock to ensure that each thread's operation on the object obtains the correct result.

### 116. Tell me about the following concepts: synchronous, asynchronous, blocking, non-blocking?
Synchronization: Multiple tasks are executed in sequence, and the next can be executed after one is executed.

Asynchronous: There is no sequence between multiple tasks and can be executed at the same time. Sometimes a task may need to obtain the result of another task executed at the same time when necessary. This is called a callback!

Blocking: If the caller is stuck, the caller cannot continue to execute, that is, the caller is blocked.

Non-blocking: If you don't get stuck, you can continue execution, that is, non-blocking.

Synchronous and asynchronous are relative to multitasking, and blocking and non-blocking are relative to code execution.

### 117. What are zombie processes and orphan processes? How to avoid zombie processes?
Orphan process: The parent process exits and the child processes that are still running are all orphan processes. The orphan process will be adopted by the init process (process number 1), and the init process will complete the status collection work for them.

Zombie process: The process uses fork to create a child process. If the child process exits and the parent process does not call wait to obtain waitpid to obtain the status information of the child process, then the process descriptor of the child process is still stored in the system. These processes are zombie processes.

Ways to avoid zombie processes:

1. Fork twice use the grandchild process to complete the task of the child process

2. Use the wait() function to block the parent process

3. Use the semaphore, call waitpid in the signal handler, so that the parent process does not need to be blocked
### 118. What are the usage scenarios of processes and threads in python?
Multi-process is suitable for CPU-intensive operations (cpu operation instructions are more, such as floating-point operations with more bits).

Multithreading is suitable for IO dense operations (read and write data operations are more than that, such as crawlers)

### 119. Are threads concurrent or parallel, and are processes concurrent or parallel?
Threads are concurrent and processes are parallel;

Processes are independent of each other and are the smallest unit for the system to allocate resources. All threads in the same thread share resources.

### 120. Parallel (parallel) and concurrency (concurrency)?
Parallel: multiple tasks are running at the same time

Will not run at the same time at the same time, there is a case of alternate execution.

The libraries that implement parallelism are: multiprocessing

Libraries that implement concurrency are: threading

Programs that need to perform more read and write, request and reply tasks require a lot of IO operations, and IO-intensive operations use concurrency better.

For programs with a large amount of CPU calculations, it is better to use parallelism
### 121. What is the difference between IO-intensive and CPU-intensive?
IO intensive: The system is running, most of the conditions are CPU waiting for I/O (hard disk/memory) read/write

CPU-intensive: Most of the time is used to do calculations, logic judgments and other CPU actions are called CPU-intensive.
### 122. How does python asyncio work?
The asyncio library is to use python's yield, a mechanism that can interrupt the context of saving the current function, encapsulate the selector and get rid of the complex callback relationship

## network programming
### 123. How to forcibly close the connection between the client and the server?
### 124. Briefly describe the difference, advantages and disadvantages of TCP and UDP?
### 125. Briefly describe the process of the browser requesting dynamic resources through WSGI?
The request sent by the browser is monitored by Nginx. Nginx distributes the requested static resource to the static resource directory according to the PATH or suffix of the requested URL, and other requests are forwarded to the corresponding port according to the configured.
A program that implements WSGI will listen to a certain port. After receiving the request forwarded by Nginx (usually use socket recv to receive HTTP messages), the requested message will be encapsulated into a dictionary object of `environ`, and then Provide a `start_response` method. Pass these two objects as parameters to a method such as `wsgi_app(environ, start_response)` or implement an instance of the `__call__(self, environ, start_response)` method. This instance calls `start_response` to return to the middleware that implements WSGI, and then the middleware returns to Nginx.
### 126. Describe the process of visiting www.baidu.com with a browser
### 127. The difference between Post and Get requests?
### 128. The difference between cookie and session?
### 129. List the status codes of the HTTP protocol you know, and what do they mean?
### 130. Please briefly talk about the three handshake and the four wave of hands?
### 131. Tell me what is 2MSL of tcp?
### 132. Why must the client wait for 2MSL in the TIME-WAIT state?
### 133. Tell me about the difference between HTTP and HTTPS?
### 134. Talk about the HTTP protocol and the fields that indicate the data type in the protocol header?
### 135. What are the HTTP request methods?
### 136. What parameters need to be passed in to use Socket?
### 137. Common HTTP request headers?
### 138. Seven-layer model?
### 139. The form of the url?

# Web
## Flask
### 140. Understanding of Flask Blueprint?
Definition of blueprint

Blueprint/Blueprint is a method of componentization of Flask applications. Blueprints can be shared within an application or across multiple projects. Using blueprints can greatly simplify the development of large-scale applications, and also provides a centralized mechanism for Flask extensions to register services in applications.

Application scenarios of the blueprint:

Decompose an application into a collection of blueprints. This is ideal for large applications. A project can instantiate an application object, initialize several extensions, and register a collection of blueprints.

Register a blueprint on the app with URL prefix and/or subdomain. The parameters in the URL prefix/subdomain name become the common view parameters of all view functions under this blueprint (by default)
Register a blueprint multiple times with different URL rules in an application.

Provide template filters, static files, templates, and other functions through blueprints. A blueprint does not have to implement application or view functions.

When initializing a Flask extension, register a blueprint in these situations.

Disadvantages of blueprints:

You cannot deregister a blueprint after the application is created without destroying the entire application object.

Three steps to use blueprints

1. Create a blueprint object
```python
blue = Blueprint("blue",__name__)
```
2. Perform operations on this blueprint object, such as registering routes, specifying static folders, registering template filters...
```python
@blue.route('/')
def blue_index():
    return "Welcome to my blueprint"
```
3. Register the blueprint object on the application object
```python
app.register_blueprint(blue,url_prefix="/blue")
```

### 141. The difference between Flask and Django routing mapping?
  In django, routing is the url in the project that the browser visits first when the browser accesses the server, and then the url in the project is used to find the url in the application. These urls are placed in a list and follow the rule of matching from front to back. In flask, routing is provided to each view function through a decorator, and a URL can be used for different functions depending on the request method.

## Django
### 142. What is wsgi, uwsgi, uWSGI?
WSGI:

The web server gateway interface is a set of protocols. Used to receive user requests and encapsulate the request for the first time, and then hand the request to the web framework.

The module that implements the wsgi protocol: wsgiref, essentially writing a socket server to receive user requests (django)

werkzeug, essentially writing a socket server to receive user requests (flask)

uwsgi:

It is a communication protocol like WSGI. It is an exclusive protocol of the uWSGI server and is used to define the type of transmission information.
uWSGI:

It is a web server that implements the WSGI protocol, uWSGI protocol, and http protocol

### 143. Comparison of Django, Flask, Tornado?
1. Django takes a broad and comprehensive direction and has high development efficiency. Its MTV framework, built-in ORM, admin background management, built-in sqlite database and server for development and testing, have improved the developer's ultra-high development efficiency.
A heavyweight web framework with complete functions and a one-stop solution, so that developers do not need to spend a lot of time on selection.

Comes with ORM and template engine, supports unofficial template engines such as jinja.

The built-in ORM makes Django and the relational database highly coupled. If you want to use a non-relational database, you need to use a third-party library

Built-in database management app

Mature, stable, and highly efficient in development. Compared with Flask, Django has better overall closedness and is suitable for enterprise-level website development. Pioneer of python web framework, rich third-party libraries

2. Flask is a lightweight framework, free, flexible, and extensible. The core is based on Werkzeug WSGI tool and jinja2 template engine

It is suitable for small websites and web service APIs, there is no pressure to develop large websites, but the architecture needs to be designed by yourself

The combination with relational databases is not weaker than Django, and the combination with non-relational databases is far superior to Django

3. Tornado is taking a small but precise direction, with superior performance, its most famous asynchronous non-blocking design method

Two core modules of Tornado:

iostraem: Simple encapsulation of non-blocking sockets

ioloop: A encapsulation of I/O multiplexing, which implements a singleton

### 144. The difference between CORS and CSRF?
What is CORS?

CORS is a W3C standard, the full name is "Cross-origin resoure sharing" (Cross-origin resoure sharing).
It allows browsers to send XMLHttpRequest requests to cross-origin servers, thereby overcoming the restriction that AJAX can only be used from the same source.

What is CSRF?

The mainstream CSRF defense method is to generate a string of random tokens when the form is generated on the backend, which is built into the form as a field, and at the same time, this string of tokens is placed in the session. Each time the form is submitted to the backend, it will check whether these two values ​​are the same to determine whether the form submission is credible. After one submission, if the page does not generate a CSRF token, the token will be cleared. , If there is a new demand, then the token will be updated.
An attacker can fake a POST form submission, but he does not have a token built into the form generated by the backend, and no token in the session will not help.

### 145.Session, Cookie, JWT Understanding
Why use session management

As we all know, the HTTP protocol is a stateless protocol, which means that each request is an independent request, and there is no relationship between the request and the request. But in actual application scenarios, this approach does not meet our needs. For an example that everyone likes to use, add a product to the shopping cart, and consider this request separately. The server does not know who owns the product, and whose shopping cart should be added? Therefore, the context of this request should actually contain user-related information. Each time the user makes a request, this small amount of additional information is also included as part of the request, so that the server can target specific information based on the information in the context. Of users to operate. Therefore, the emergence of these several technologies is a supplement to the HTTP protocol, so that we can use HTTP protocol + state management to build a user-oriented WEB application.

The difference between Session and Cookie

  Here I want to talk about session and cookies first, because these two technologies are the most common for development. So what is the difference between session and cookies? I personally think that the core difference between session and cookies is who maintains the additional information. When cookies are used to implement session management, user-related information or other information we want to keep in each request is placed in cookies, and cookies are saved by the client, whenever the client sends a new request , It will bring cookies a little, and the server will operate according to the information in them.
  When using session for session management, the client actually only saves a session_id sent by the server, and from this session_id, all the state information needed can be restored on the server. From here, it can be seen that this part of the information is Maintained by the server.

In addition, sessions and cookies have some disadvantages of their own:
    
The security of cookies is not good. Attackers can deceive by obtaining local cookies or use cookies to conduct CSRF attacks. When cookies are used, there will be cross-domain issues under multiple domain names.
The session needs to be stored on the server for a certain period of time. Therefore, when there are a large number of users, the performance of the server will be greatly reduced. When there are multiple machines, how to share the session will also be a problem. (redis cluster) also That is to say, the first time the user visits is server A, and the second request is forwarded to server B, how does server B know its status? In fact, session and cookies are related, for example, we can store session_id in cookies.

How does JWT work

First, the user sends a login request, and the server performs matching according to the user's login request. If the matching is successful, put the relevant information into the payload, use the algorithm, plus the server's key to generate the token. It is important to note here that the secret_key is very important. If this is leaked, the client can randomly tamper with the additional information sent, which is a guarantee of the integrity of the information. After the token is generated, the server returns it to the client, and the client can pass the token to the server in the next request. Generally speaking, we can put it in the Authorization header, so that cross-domain problems can be avoided.

### 146. Briefly describe the Django request life cycle
Generally, the user initiates a request to our server through the browser. This request will access the view function. If there is no data call involved, then the view function returns a template that is a web page to the user at this time)
The view function calls the model hair model to find the data in the database, and then returns step by step. The view function fills the returned data into the blanks in the template, and finally returns the web page to the user.

1.wsgi, the request is encapsulated and handed over to the web framework (Flask, Django)

2. Middleware, to verify the request or add other relevant data to the request object, for example: csrf, request.session

3. Route matching according to the different URL sent by the browser to match different view functions

4. View function, the processing of business logic in the view function, may involve: orm, templates

5. Middleware to process the response data

6.wsgi, send the content of the response to the browser

### 147. Use restframework to complete the api sending time and time zone
The current problem is to use django's rest framework module to make a get request sending time and time zone information api
```python
class getCurrenttime(APIView):
    def get(self,request):
        local_time = time.localtime()
        time_zone =settings.TIME_ZONE
        temp = {'localtime':local_time,'timezone':time_zone}
        return Response(temp)
```
### 148. What are nginx, tomcat and apach?
Nginx (engine x) is a high-performance HTTP and reverse proxy server. It is also an IMAP/POP3/SMTP server. It works at OSI seven layers. The load implementation method: polling, IP_HASH, fair, session_sticky.
Apache HTTP Server is a modular server, derived from the NCSAhttpd server
Tomcat server is a free and open source web application server, which is a lightweight application server and is the first choice for developing and debugging JSP programs.

### 149. What are the paradigms of relational database you are familiar with, and what are their functions?
When designing a database, you can design a database structure without data redundancy and abnormal data maintenance as long as you design in accordance with the design specifications.

There are many specifications for database design. Generally speaking, when we set up a database, we only need to meet some of these specifications. These specifications are also called the three paradigms of databases. There are three in total, and there are other paradigms. We just need to do To meet the requirements of the first three paradigms, we can set up a database that conforms to ours. We can't all follow the requirements of the paradigm, but also consider the actual business usage, so sometimes we need to do something that violates the paradigm. Requirements.
1. The first paradigm of database design (the most basic). Basically all database paradigms conform to the first paradigm. The tables that conform to the first paradigm have the following characteristics:

All fields in the database table have only a single attribute. The columns of a single attribute are composed of basic data types (integer, floating point, character, etc.). The designed tables are simple two-comparison tables

2. The second paradigm of database design (designed on the basis of the first paradigm) requires only one business primary key in a table, which means that there can be no non-primary key column pairs in the second paradigm. Dependency of the primary key

3. The third paradigm of database design means that every non-primary attribute is neither partially dependent nor transitively dependent on the business primary key, which is based on the second paradigm, eliminating the transitive dependence of non-primary attributes on the primary key

### 150. Briefly describe the QQ login process
QQ login is divided into three interfaces in our project,

The first interface is to request the QQ server to return a QQ login interface;

The second interface is to verify by scanning code or account login. The QQ server returns a code and state to the browser. Use this code to get the access_token from the QQ server through the local server, and then return it to the local server, and then get the user from the QQ server with the access_token. Openid (unique identifier of openid user)

The third interface is to determine whether the user is logging in to QQ for the first time, if not, log in the returned jwt-token directly to the user, and for users who have not been bound to this website, encrypt the openid to generate the token for binding

### 151. What is the difference between post and get?
1. GET is to get data from the server, POST is to send data to the server

2. On the client side, the GET method is to submit the data through the URL, the data can be seen in the URL, and the POST method, the data is placed in HTML-HEADER to submit

3. For the GET method, the server side uses Request.QueryString to obtain the value of the variable. For the POST method, the server side uses Request.Form to obtain the submitted data.


### 152. The role of the log in the project
1. Log related concepts

1. Logs are a way to track events that occur when certain software is running

2. Software developers can call logging-related methods into their code to indicate that something has happened

3. An event can be described by a message containing optional variable data

4. In addition, events also have the concept of importance, which can also be called severity level (level)

Second, the role of the log

1. Through log analysis, it is convenient for users to understand the operation of the system, software, and application;

2. If your application log is rich enough, you can analyze past user behavior, type preferences, geographic distribution or more information;

3. If the log of an application is divided into multiple levels at the same time, the health status of the application can be easily analyzed, problems can be discovered in time, and problems can be quickly located, solved, and remedied.

4. Simply speaking, we can understand whether a system or software program is operating normally by recording and analyzing logs, and can also quickly locate problems when an application fails. Logs are also very important not only in development, but also in operation and maintenance, and the role of logs can also be simple. Summarized as the following points:

1. Program debugging

2. Understand the operation of the software program, whether it is normal

3. Software program operation failure analysis and problem location

4. If the log information of the application is sufficiently detailed and rich, it can also be used for user behavior analysis

### 153. How to use django middleware?
Django presets six methods in the middleware. The difference between these six methods is that they are executed in different stages and intervene in input or output. The methods are as follows:

1. Initialization: without any parameters, it is called once when the server responds to the first request to determine whether to enable the current middleware
```python
def __init__():
    pass
```
2. Before processing the request: call on each request and return None or HttpResponse object.
```python
def process_request(request):
    pass
```
3. Before processing the view: call on each request, return None or HttpResponse object.
```python
def process_view(request,view_func,view_args,view_kwargs):
    pass
```
4. Before processing the template response: call on each request, and return the response object that implements the render method.
```python
def process_template_response(request,response):
    pass
```
5. After processing the response: All responses are called before returning to the browser, called on each request, and the HttpResponse object is returned.
```python
def process_response(request,response):
    pass
```
6. Exception handling: called when the view throws an exception, called on each request, and returns an HttpResponse object.
```python
def process_exception(request,exception):
    pass
```
### 154. Tell me about your understanding of uWSGI and nginx?
1. uWSGI is a web server, which implements the WSGI protocol, uwsgi, http and other protocols. The role of HttpUwsgiModule in Nginx is to exchange with uWSGI server. WSGI is a web server gateway interface. It is a specification for communication between a web server (such as nginx, uWSGI, etc.) and web applications (such as programs written in the Flask framework).

Pay attention to the distinction between the three concepts of WSGI/uwsgi/uWSGI.

WSGI is a communication protocol.

uwsgi is a wire protocol rather than a communication protocol. It is often used here for data communication between the uWSGI server and other network servers.

uWSGI is a web server that implements both uwsgi and WSGI protocols.

nginx is an open source high-performance HTTP server and reverse proxy:

1. As a web server, it handles static files and index files very efficiently

2. Its design pays great attention to efficiency, supports up to 50,000 concurrent connections, but only takes up very little memory space

3. High stability and simple configuration.

4. Powerful reverse proxy and load balancing function, balance the load pressure application of each server in the cluster

### 155. What are the application scenarios of the three major frameworks in Python?
Django: It is mainly used for rapid development. Its highlight is rapid development and cost saving. If high concurrency is to be achieved, Django must be developed twice, such as removing the entire bulky framework and writing sockets by yourself. To achieve http communication, the bottom layer is written in pure c, c++ to improve efficiency, the ORM framework is killed, and the framework that encapsulates the interaction with the database is written by yourself. Although the ORM is object-oriented to operate the database, its efficiency is very low, and the foreign key is used to contact the table. Query with the table;
Flask: Lightweight, it is mainly used to write a framework for the interface, to achieve the separation of front and back ends, and to test the development efficiency. Flask itself is equivalent to a core, and almost all other functions need to be extended (mail extension Flask-Mail, User authentication (Flask-Login), all need to be implemented with third-party extensions. For example, you can use Flask-extension to join ORM, file upload, identity verification, etc. Flask does not have a default database. You can choose MySQL or NoSQL.

Its WSGI toolbox uses Werkzeug (routing module), and its template engine uses Jinja2. These two are also the core of the Flask framework.

Tornado: Tornado is an open source version of web server software. Tornado is obviously different from current mainstream web server frameworks (including most Python frameworks): it is a non-blocking server, and it is quite fast. Thanks to its non-blocking method and the use of epoll, Tornado can handle thousands of connections per second, so Tornado is an ideal framework for real-time web services
### 156. Where are threads used in Django? Where is the coroutine used? Where is the process used?
1. Time-consuming tasks in Django are executed by a process or thread, such as sending emails, using celery.

2. It is time to deploy the django project, and the relevant configuration of the process and the coroutine is set in the configuration file.

### 157. Have you ever used Django REST framework?
Django REST framework is a powerful and flexible Web API tool. The reasons for using RESTframework are:

Web browsable API has great benefits for developers

Including OAuth1a and OAuth2 authentication strategies

Support serialization of ORM and non-ORM data resources

Full custom development-if you don't want to use more powerful functions, you can just use regular function-based views, additional documentation and strong community support
### 158. Know about cookies and session? Can they be used alone?
Session adopts the scheme of keeping state on the server side, and Cookie adopts the scheme of keeping state on the client side. But if you disable cookies, you cannot get the Session. Because Session uses Session ID to determine the server Session corresponding to the current session, and Session ID is passed through Cookie, disabling Cookie is equivalent to SessionID, so Session cannot be obtained.

## Crawler
### 159. Try to list at least three currently popular large databases
### 160. List the network packets used by the Python web crawler you have used?

requests, urllib,urllib2, httplib2

### 161. Which database is used to store the data after crawling the data, and why?

### 162. What crawler frameworks or modules have you used? Pros and cons?

Python comes with: urllib, urllib2

Third party: requests

Framework: Scrapy

Both the urllib and urllib2 modules do operations related to requesting URLs, but they provide different functions.

urllib2: urllib2.urlopen can accept a Request object or url, (when receiving a Request object, and use this to set a URL header), urllib.urlopen only accepts a url.

urllib has urlencode, urllib2 does not, so it is always the reason why urllib and urllib2 are often used together

Scrapy is a packaged framework. It includes downloader, parser, log and exception handling. It is based on multi-threaded and twisted processing. It has advantages for crawling development of a fixed single website, but it can crawl 100 for multiple websites. The website, concurrent and distributed processing is not flexible enough, and it is inconvenient to adjust and expand

requests is an HTTP library, it is only used for requests, it is a powerful library, downloading and parsing are all handled by themselves, with high flexibility

Scrapy advantages: asynchronous, xpath, powerful statistics and log system, support for different URLs. The shell is convenient for independent debugging. Write middleware to facilitate filtering. Stored in the database through the pipeline

### 163. Is it better to use multiple processes to write crawlers? Is multithreading better?
### 164. Common anti-reptiles and countermeasures?
### 165. Which are the most used parsers for parsing web pages?
### 166. How to solve the problem of restricting ip, cookie, session at the same time for web pages that need to log in
### 167. How to solve the verification code?
### 168. What do you understand about the most used databases?
### 169. Which crawler middleware have you written?
### 170. How to crack the "JiYi" sliding verification code?
### 171. How often does the crawler crawl, and how is the data stored?
### 172. How to deal with cookie expiration?
### 173. How to deal with dynamic loading and high requirements for timeliness?
### 174. What are the advantages and disadvantages of HTTPS?
### 175. How does HTTPS realize secure data transmission?
### 176. What are TTL, MSL and RTT?
### 177. Talk about your understanding of Selenium and PhantomJS
### 178. How do you usually use a proxy?
### 179. Stored in the database (redis, mysql, etc.).
### 180. How to monitor the status of crawlers?
### 181. Describe the mechanism of scrapy framework operation?
### 182. Talk about your understanding of Scrapy?
### 183. How to make the scrapy framework send a post request (write it out)
### 184. How to monitor the status of crawlers?
### 185. How to judge whether the website is updated?
### 186. How to bypass the anti-theft connection when crawling pictures and videos
### 187. How large is the amount of data you crawled out of? How often does it take to climb?
### 188. What data inventory is used to climb down the data? Did you do the deployment? How to deploy?
### 189. Incremental crawling
### 190. How to de-duplicate the crawled data, and talk about the specific algorithm basis of scrapy.
### 191. What are the advantages and disadvantages of Scrapy?
### 192. How to set the crawl depth?
### 193. What is the difference between scrapy and scrapy-redis? Why choose redis database?
### 194. What problem does distributed crawler mainly solve?
### 195. What is distributed storage?
### 196. What distributed crawler solutions do you know?
### 197.scrapy-redis, have you done other distributed crawlers?

# Database
## MySQL
### 198. Primary key Super key Candidate key Foreign key

Primary key: A combination of data columns or attributes in a database table that uniquely and completely identify the stored data object. A data column can only have one primary key, and the value of the primary key cannot be missing, that is, it cannot be a null value (Null).

Super key: The set of attributes that can uniquely identify the tuple in the relationship is called the super key of the relationship mode. An attribute can be used as a super key, and multiple attributes can also be used as a super key. Super keys include candidate keys and primary keys.

Candidate key: It is the smallest super key, that is, the super key without redundant elements.

Foreign key: The primary key of another table that exists in one table is called the foreign key of this table.

### 199. The role of the view, can the view be changed?

Views are virtual tables, which are not the same as tables that contain data. Views only contain queries that dynamically retrieve data when used; they do not contain any columns or data. Using views can simplify complex SQL operations, hide specific details, and protect data; after views are created, they can be used in the same way as tables.

The view cannot be indexed, nor can it have associated triggers or default values. If there is an order by in the view itself, the order by of the view will be overwritten again.

Create a view: create view xxx as xxxxxx

For some views, such as the grouping aggregate function Distinct Union that does not use join subqueries, it can be updated. The update of the view will update the base table; but the view is mainly used to simplify retrieval and protect data, and is not used for updating , And most views cannot be updated.

### 200. The difference between drop, delete and truncate

Drop directly deletes the table, truncate deletes the data in the table, and then inserts the auto-increment id from 1 again, delete deletes the data in the table, you can add the word where.

1. The delete statement executes the delete process to delete a row from the table each time, and at the same time the delete operation of the row is recorded as a transaction and saved in the log for rollback operation. Truncate table deletes all data from the table at one time and does not record a separate delete operation record into the log for storage. Deleted rows cannot be recovered. And the delete trigger related to the table will not be activated during the delete process, and the execution speed is fast.

2. The space occupied by tables and indexes. When the table is truncate, the space occupied by the table and index will be restored to the initial size, and the delete operation will not reduce the space occupied by the table or index. The drop statement releases all the space occupied by the table.

3. Generally speaking, drop>truncate>delete

4. The scope of application. Truncate can only be table, delete can be table and view

5.truncate and delete only delete data, while drop deletes the entire table (structure and data)

6.truncate and delete without where: only delete data, without deleting the structure (definition) of the table. The drop statement will delete the constraint (constrain), trigger (trigger) index (index) on which the structure of the table is dependent; depends on The stored procedure/function of the table will be retained, but its status will become: invalid.

### 201. The working principle and types of indexes

The database index is a sorted data structure in the database management system to assist in quick query and update the data in the database table. The realization of the index usually uses the B tree and its variant B+ tree.

In addition to data, the database system also maintains data structures that meet specific search algorithms. These data structures reference (point to) data in a certain way, so that advanced search algorithms can be implemented on these data structures. This data structure is the index.

There is a price to pay for setting up an index for the table: one is to increase the storage space of the database, and the other is to spend more time when inserting and modifying data (because the index will also change accordingly)
### 202. Connection type
### 203. Thoughts on Database Optimization
### 204. The difference between stored procedures and triggers
### 205. What are pessimistic locks and optimistic locks?
### 206. What are your commonly used mysql engines? What are the differences between the engines?

## Redis
### 207. How to solve Redis downtime?

Downtime: The server is out of service'

If there is only one redis, it will definitely cause data loss and cannot be saved

For multiple redis or redis clusters, downtime needs to be divided into master-slave mode:

The slave is down from redis, and the slave redis is configured when the master-slave replication is configured. The slave will read the master redis operation log 1 from the master redis. After the slave library restarts in the redis, it will automatically be added to the master-slave In the architecture, the synchronization of data is automatically completed;

2, If the slave database is persisted, do not restart the service immediately at this time, otherwise it may cause data loss. The correct operation is as follows: execute SLAVEOF ON ONE on the slave data to disconnect the master-slave relationship and upgrade the slave As the master database, restart the master database at this time, execute SLAVEOF, set it as a slave database, connect to the master redis for master-slave replication, and automatically back up data.

The above process is easy to configure errors, you can use the sentinel mechanism provided by redis to simplify the above operations. The simple way: the function of the sentinel of redis

### 208. The difference between redis and mecached, and usage scenarios

the difference

1. Both redis and Memcache store data in memory, and both are memory databases. But memcache can also be used to cache other things, such as pictures, videos, etc.

2. Redis not only supports simple k/v type data, but also provides storage for list, set, hash and other data structures

3. Virtual memory-redis When the logistics memory is used up, some values ​​that have not been used for a long time can be exchanged to disk

4. Expiration policy-memcache is specified when set, such as set key1 0 0 8, which means it will never expire. Redis can be set by, for example, expire, such as expire name 10

5. Distributed-set up a memcache cluster, use magent to do one master and multiple slaves, redis can do one master and multiple slaves. Can be one master and one cluster

6. Store data security-After memcache hangs, the data is gone, redis can be saved to disk regularly (persistence)

7. Disaster recovery-data cannot be recovered after memcache is down, redis data can be recovered by aof after data loss

8. Redis supports data backup, that is, data backup in master-slave mode

9. The application scenarios are different. In addition to being used as a NoSQL database, redis can also be used as a message queue, data stack, and data cache; Memcache is suitable for caching SQL statements, data sets, temporary user data, delayed query data and session, etc.

scenes to be used

1. If you have long-lasting requirements or have requirements for data types and processing, you should choose redis

2. If simple key/value storage, you should choose memcached.

### 209. How to do the Redis cluster solution? What are the solutions?

1, codis

The most commonly used cluster solution at present has basically the same effect as twemproxy, but it supports the restoration of data from the old node to the new hash node when the number of nodes changes.

2 The cluster that comes with redis cluster3.0 is characterized in that its distributed algorithm is not a consistent hash, but the concept of a hash slot, and its own support for node setting slave nodes. See the official introduction for details

3. Realize in the business code layer, set up several unrelated redis instances, in the code layer, perform hash calculation on the key, and then go to the corresponding redis instance to manipulate the data. This method has relatively high requirements for the hash layer code. Some considerations include alternative algorithm schemes after node failure, dictionary script recovery after data shock, instance monitoring, etc.

### 210. How does the Redis recycling process work?

A client ran a new command and added new data.

Redis checks the memory usage, and if it is greater than the maxmemory limit, it will be recycled according to the set strategy.

A new command is executed and so on, so we are constantly crossing the boundary of the memory limit, by continuously reaching the boundary and then continuously reclaiming back below the boundary.

If the result of a command causes a large amount of memory to be used (for example, the intersection of a large set is saved to a new key), it will not take long for the memory limit to be exceeded by this memory usage.

## MongoDB
### 211. What is the command to update multiple records in MongoDB?
### 212. How does MongoDB expand to multiple shards?

## Test
### 213. The purpose of writing a test plan is
### 214. Test the keyword trigger module
### 215. Summary of other commonly used written exam URLs
### 216. What are the tasks of testers in the software development process
### 217. What is included in a software bug record?
### 218. Briefly describe the advantages and disadvantages of black box testing and white box testing
### 219. Please list the types of software testing you know, at least 5 items
### 220. What is the difference between Alpha test and Beta test?
### 221. Give examples to illustrate what is a bug? What keywords should a bug report contain?

## data structure
### 222. Numbers that appear more than half the number of times in the array-Python version
### 223. Find prime numbers within 100
### 224. The longest substring without repeated characters-Python implementation
### 225. Get 3 liters of water from the pond through 2 5/6 liter kettles
### 226. What is MD5 encryption and what are its characteristics?
### 227. What is symmetric encryption and asymmetric encryption
### 228. The idea of ​​bubble sorting?
### 229. The idea of ​​quick sort?
### 230. How to judge whether there is a ring in a singly linked list?
### 231. Which sorting algorithm do you know (usually through the question test algorithm)
### 232. Fibonacci Sequence

**Sequence definition: **

f 0 = f 1 = 1
f n = f (n-1) + f (n-2)

#### By definition

The speed is very slow, in addition (Attention to the violent stack! ⚠️️) `O(fibonacci n)`

```python
def fibonacci(n):
    if n == 0 or n == 1:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)
```

#### Linear time

**Status/Circulation**

```python
def fibonacci(n):
   a, b = 1, 1
   for _ in range(n):
       a, b = b, a + b
   return a
```

**Recursion**

```python
def fibonacci(n):
    def fib(n_, s):
        if n_ == 0:
            return s[0]
        a, b = s
        return fib(n_-1, (b, a + b))
    return fib(n, (1, 1))
```

**map(zipwith)**

```python
def fibs():
    yield 1
    fibs_ = fibs()
    yield next(fibs_)
    fibs__ = fibs()
    for fib in map(lambad a, b: a + b, fibs_, fibs__):
        yield fib
        
        
def fibonacci(n):
    fibs_ = fibs()
    for _ in range(n):
        next(fibs_)
    return next(fibs)
```

**Do caching**

```python
def cache(fn):
    cached = {}
    def wrapper(*args):
        if args not in cached:
            cached[args] = fn(*args)
        return cached[args]
    wrapper.__name__ = fn.__name__
    return wrapper

@cache
def fib(n):
    if n <2:
        return 1
    return fib(n-1) + fib(n-2)
```

**Use funtools.lru_cache for caching**

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n <2:
        return 1
    return fib(n-1) + fib(n-2)
```

#### Logarithmic

**matrix**

```python
import numpy as np
def fibonacci(n):
    return (np.matrix([[0, 1], [1, 1]]) ** n)[1, 1]
```

**Not a matrix**

```python
def fibonacci(n):
    def fib(n):
        if n == 0:
            return (1, 1)
        elif n == 1:
            return (1, 2)
        a, b = fib(n // 2-1)
        c = a + b
        if n% 2 == 0:
            return (a * a + b * b, c * c-a * a)
        return (c * c-a * a, b * b + c * c)
    return fib(n)[0]
```

### 233. How to flip a singly linked list?

```python
class Node:
    def __init__(self,data=None,next=None):
        self.data = data
        self.next = next
        
def rev(link):
    pre = link
    cur = link.next
    pre.next = None
    while cur:
        temp = cur.next
        cur.next = pre
        pre = cur
        cur = tmp
    return pre

if __name__ =='__main__':
    link = Node(1,Node(2,Node(3,Node(4,Node(5,Node(6,Node7,Node(8.Node(9))))))))
    root = rev(link)
    while root:
        print(roo.data)
        root = root.next
```



### 234. The problem of frog jumping

A frog wants to jump up n-level steps. It can jump one level or two at a time. How many ways does this frog have to jump up this n-level step?

Method 1: Recursion

Suppose there are f(n) ways for a frog to jump on n steps. These n methods are divided into two categories. The first one jumps one step last time. There are f(n-1) kinds of this kind, and the second This method jumped two steps at the last time. There are f(n-2) kinds of this method, and the recursive formula f(n)=f(n-1) + f(n-2) is obtained. Obviously f(1 )=1, f(2)=2. Although this method is simple in code, it is inefficient and will exceed the time limit

```python
class Solution:
    def climbStairs(self,n):
        if n == 1:
            return 1
        elif n==2:
            return 2
        else:
            return self.climbStairs(n-1) + self.climbStairs(n-2)
```

Method 2: Use loops instead of recursion

```python
class Solution:
    def climbStairs(self,n):
        if n==1 or n==2:
            return n
        a,b,c = 1,2,3
        for i in range(3,n+1):
            c = a+b
            a = b
            b = c
        return c
```

### 235. Two Sum Two Sum



### 236. Search in Rotated Sorted Array Search in Rotated Sorted Array
### 237. Python implements a Stack data structure
### 238. Write a binary search
### 239. What is the time complexity of using in for set and why?
### 240. There are n positive integers in the range of [0, 1000] in the list, sorted;
### 241. There are methods of composition and inheritance in object-oriented programming to implement new classes
## Big Data
### 242. Find out high-frequency words in 1G files
### 243. Count high-frequency words in a text file of about ten thousand lines
### 244. How to find the most repeated one among the massive data?
### 245. Determine whether the data is in a large amount of data

## Architecture

### [Python back-end architecture evolution](<https://zhu327.github.io/2018/07/19/python%E5%90%8E%E7%AB%AF%E6%9E%B6%E6% 9E%84%E6%BC%94%E8%BF%9B/>)

This article almost covers the architecture that python will use. In the interview, you can draw the architecture diagram by hand, and talk about the technical selection and pros and cons according to your own project, and the pits you encounter. Absolute bonus.

## CREDITS

Original Credits: [kenwoodjw](https://github.com/kenwoodjw)

English Credits: [jishanshaikh4](https://github.com/jishanshaikh4)


