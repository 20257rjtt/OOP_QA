#  基本调试想法

​        多打印日志方便调试，在每种情况里打印对应该情况的信息，有利于理解程序到底是按什么顺序执行的，以及到底进入的是哪种情况里。（特别是在程序实际的执行顺序与我们的设想不一致的时候）

# 错误代码记录

## 错误1

1、原始代码如下：

```c++
//得到实际变量个数
int SubSentence::get_exist_number() {
  for (int i = 0; i < variable_number; ++i) {
    if (subsentence[i] != 0) {
      ++exist_variable_number;
    }
  }
  return exist_variable_number;
}
```

2、错误说明：在执行时没有首先将exist_variable_number初始化为0，导致每调用一次就会在之前的基础上继续++，得到的便不是真实的值了。

3、改正代码如下：

```c++
//使exist_number与子句匹配
void SubSentence::check_exist_number()
{
  exist_variable_number=0;//先初始化
  for (int i = 0; i < variable_number; ++i) {
    if (subsentence[i] != 0) {
      ++exist_variable_number;
    }
  }
}

//得到实际变量个数
int SubSentence::get_exist_number() {
  check_exist_number();//首先判断成员变量exist_number是否与存储的子句信息匹配
  return exist_variable_number;
}

```

## 错误2

1、原始代码如下：

```c++
//判断是否对某个变量是否有冲突的单子句，传入变量的指标
bool CNF::conflict_simpleSubsentence(int _index) {
  if (state_true(_index) == true && state_false(_index) == false) {
    return true;
  }
  return false;
}
```

2、错误说明：state_false()函数返回true时说明有存在第_index个变量为“非”的单子句

所以if里面第二个判断应该是==true

3、改正代码如下：

```c++
//判断是否对某个变量是否有冲突的单子句，传入变量的指标
bool CNF::conflict_simpleSubsentence(int _index) {
  if (state_true(_index) == true && state_false(_index) == true) {//这里一开始写成了“state_false(_index)==false"自然不对！！！
    return true;
  }
  return false;
}
```

