# Python Interview Questions 1

### Date

September 25, 2020 

## Movivation 

Recently I had two interview with python related positions. I believe it's worthy to record it. I can't remember everything in detail, so I just try to tell the story in my way. If you find something wrong, please let me know. Thanks!

## Questions

1. Please write down the result.
   
   ```python
   class A:
    x = 5
    def __init__(self):
        self.y = 1

    class B(A):
    x = 6

    class C(A):
        pass

    A.x = 7
    B.x = 8
    a = A()
    a.y = 2
    b = B()
    b.y = 3
    c = C()

    print(A.x, B.x, C.x)
    print(a.y, b.y, c.y)
   ```

   Ans: 
   ```
   7 8 7
   2 3 1
   ```

2. Please write down the result.
   
   ```python
    def func(x, dic={}):
        dic[x] = x
        return dic

    res = {}
    for i in range(10):
    res = func(i)

    ans = 0
    for v in res.values():
    ans += v

    print(ans)
   ```
   Ans: 
   ```
   45
   ```
   Explanation: The default value in funcion occupies an piece of memory when it's declared. If no argument, it would use the same object to finish the function call so as those x values are inserted into dic.

3. Design a Calculator class and implement functionality of plus, minus, multiply, and divide. Please try your best to make it handy to use. Hint: Maybe we can use it like a library.

   Ans 1: Implemete class methods.
   ```python
   class Calculator():
    @classmethod
    def plus(cls, val1, val2):
        return val1 + val2
    
    @classmethod
    def minus(cls, val1, val2):
        return val1 - val2
    
    @classmethod
    def multiply(cls, val1, val2):
        return val1 * val2
    
    @classmethod
    def divide(cls, val1, val2):
        if val2 == 0:
        raise ZeroDivisionError
        return val1 / val2
    
    ans = Calculator.plus(5, 3)
    ans = Calculator.minus(5, 3)
    ans = Calculator.multiply(5, 3)
    ans = Calculator.divide(5, 3)
   ```

   Ans 2: Rewrite class operator.
   ```python
    class Calculator():
        def __init__(self, val):
            self._value = val

        def __add__(self, item):
            return self._value + item._value

        def __sub__(self, item):
            return self._value - item._value

        def __mul__(self, item):
            return self._value * item._value
            
        def __truediv__(self, item):
            if item._value == 0:
            raise ZeroDivisionError
            return self._value / item._value

    ans = Calculator(5) + Calculator(3)
    ans = Calculator(5) - Calculator(3)
    ans = Calculator(5) * Calculator(3)
    ans = Calculator(5) / Calculator(3)
   ```

   Explanation: Actually I came up with these two ideas when I was testing. Not sure which one is better.
   
4. There is a special class below and we hope the adding value into it only via __insert__. Assume that insert function is completed. So please add something to make it raise InsertError correctly.
   
   ```python
    class InsertError(Exception):
        pass

    class Dictionary():
        # Need to be added...

    dic = Dictionary()
    dic.insert("Key1", "Value1")

    try:
        dic["Key2"] = "Value2"
    except :
        print("InsertError is caught.") 

    # result: InsertError is caught.
   ```

   Ans: 
   ```python
    class Dictionary():
        def __setitem__(self, key, value):
            raise InsertError
   ```
