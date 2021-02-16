# 逆波兰表达式

**150. 逆波兰表达式求值**

根据[ 逆波兰表示法](https://baike.baidu.com/item/%E9%80%86%E6%B3%A2%E5%85%B0%E5%BC%8F/128437)，求表达式的值。

有效的运算符包括 `+`, `-`, `*`, `/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

```text
int val(vector<string>& tokens, int& st){
    string op = tokens[st--];
    if(op.size() > 1 || op[0] >= '0' && op[0] <= '9')return stoi(op);
    int right = val(tokens, st);
    int left = val(tokens, st);
    if(op == "+")return left + right;
    else if(op == "-")return left - right;
    else if(op == "*")return left * right;
    else return left / right;
}
int evalRPN(vector<string>& tokens) {
    int st = tokens.size() - 1;
    return val(tokens, st);
}
```

