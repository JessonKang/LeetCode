## Task 2：有效的括号

	题目：给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
	
	有效字符串需满足：
		1)左括号必须用相同类型的右括号闭合。
		2)左括号必须以正确的顺序闭合。
		3)注意空字符串可被认为是有效字符串。

链接：https://leetcode-cn.com/problems/valid-parentheses/

### 思路：

​		由于输入的测试用例是个字符串，因此，肯定是存在各种括号交叉出现的情况，但是有一点一定要注意，那就是对于有效的字符串，某一段连续左括号出现完了之后，后面跟着的一个右括号，一定会和左括号序列和最后一个左括号匹配，如下图：

![](images\valid_parenthesis.png)

​			因此，可以用一个栈来存储之前出现的左括号，当出现一个右括号时，将右括号与栈顶元素进行比较，如果符合条件则将栈顶元素出栈并处理下一个字符，如果不符合条件或者栈为空，则不是有效字符串。



## 算法：

1. 初始化栈S;
2. 一次处理表达式的一个字符；
3. 如果是开括号，则入栈，意味着稍后来处理它；
4. 如果是闭括号，那么检查栈顶元素，如果栈顶元素是一个想同类型的左括号，则将它出栈，然后继续处理后续字符，否则，表达式无效；
5. 如果到最后剩下的栈中仍有元素，则意味着表达式无效；



## 代码：

```java
class Solution {
    //建立开闭括号对应的哈希表，便于处理
    private HashMap<Character,Character> mappings;
    
    public Solution(){
        this.mappings = new HashMap<Character,Character>();
        this.mappings.put(')','(');
        this.mappings.put('}','{');
        this.mappings.put(']','[');
    }
    
    public boolean isValid(String s) {
        //初始化用于保存左括号的栈
        Stack<Character> stack = new Stack<Character>();
        for(int i=0;i<s.length();i++){
            char c = s.charAt(i);
            
            //是闭括号，则进行处理
            if(this.mappings.containsKey(c)){
                char topElement = stack.empty() ? '#' : stack.pop();
                if(topElement != this.mappings.get(c)){
                    //当前闭括号与栈顶开括号不匹配，不是有效字符串
                    return false;
                }   
            }else{ //是开括号，入栈
                stack.push(c);
            }
        }
        //字符串处理完毕
        return stack.isEmpty();
    }
}
```



## 思考

1. 在这个题目中应该思考的点首先是数据的处理，即如何把测试用例存储在程序中进行处理，这里的数据是一一对应的开闭括号，同时在处理的过程中还需要进行判断是否是同类型的开闭括号，所以应该想到用Map容器。
2. 在处理字符串时如果遇到左括号，就继续取下一个字符，直到遇到右括号才开始进行处理，这里有递归的思想，所以应该想到用栈来存储之前的左括号序列；
3. 实际应用：编译器检查代码的括号匹配时。