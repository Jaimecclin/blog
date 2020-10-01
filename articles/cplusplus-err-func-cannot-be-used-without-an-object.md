# [C++] A::memberfunction cannot be used without an object

### Date

October 1, 2020 

## Introduction

Last week, I had a mission adding a callback function to the system. At first, I thought that's quite easy so I had the implementation as below, but it ran into an error…

Class A is a listener that needs to execute a callback function when A specific moment and the callback will be set by class B.

Class B has a functoon __cal__ to be executed and it will register the function to A so that it can be carried out when needed. In __cal__ function, it has to sum the arguments and the value which is calculated by __doTrick()__.  

## First implementation

### Header file:
```cpp
class A
{
public:
    A();
    function<int(int, int)> callbackFunc;
    void setCB(function<int(int, int)>);
    int doCB(int, int);
};
class B{
public:
    B(A*);
    int doTrick();
    static int cal(int, int);
};
```

### Cpp file:

```cpp
A::A()
{
}
void A::setCB(function<int(int, int)> callback)
{
    callbackFunc = callback;
}
int A::doCB(int num1, int num2)
{
    return callbackFunc(num1, num2);
}
B::B(A *a)
{
    a->setCB(cal);
}
int B::doTrick()
{
    // Do a trick here.
    return 1;
}
int B::cal(int num1, int num2)
{   
    doTrick();
    return num1 + num2;
}
```

### main.cpp

```cpp
int main()
{
   A a = A();
   B b = B(&a);
   // At a certain moment, object a would execute the callback.
   cout << a.doCB(3,5);
   
   return 0;
}
```

### Error

```
main.cpp:42:13: error: cannot call member function ‘int B::doTrick()’ without object
     doTrick();
```

## Explanation

I believe that we all know what callback is so let's skip it. As the error shows, we cannot call the member function. But why? 

That's because cal is a static function, it cannot access the B class members and functions. And how could we solve it?


## Solution

__The great Google always give us the answer.__

In my opinion, the cal function needs an instance to access B class's member functions. Fortunately, We can use __std::bind__ to wrap function so as it can complete registering a callback from a class member. 


## Solution implementation

### Header file:

Please be aware that the __cal__ function is not an static function anymore.

```cpp
class A
{
public:
    A();
    function<int(int, int)> callbackFunc;
    void setCB(function<int(int, int)>);
    int doCB(int, int);
};
class B{
public:
    B(A*);
    int doTrick();
    static int cal(int, int);
};
```
### Cpp file:
```cpp
A::A()
{
}
void A::setCB(function<int(int, int)> callback)
{
    callbackFunc = callback;
}
int A::doCB(int num1, int num2)
{
    return callbackFunc(num1, num2);
}
B::B(A *a)
{
    // Warp the class menber function as a new one. 
    auto temp = bind(&B::cal, this, placeholders::_1, placeholders::_2);
    a->setCB(temp);
}
int B::doTrick()
{
    // Do a trick here.
    return 1;
}
int B::cal(int num1, int num2)
{   
    return doTrick() + num1 + num2;
}
```

### Reference 
1. https://en.cppreference.com/w/cpp/utility/functional/bind
2. https://stackoverflow.com/questions/14189440/c-callback-using-class-member