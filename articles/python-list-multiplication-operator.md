# Python list multiplication operator trap

__Technique record__

Python version: 3.6

This afternoon I sat in a cafe and coded as usual. I tried to quickly solve Medium [59. Spiral Matrix II](https://hackmd.io/SkU4C895T2Osvhufzqdt-A) but got stuck for a while. And then found that I made a common mistake so as I need to rocord it!  

In this question, I want to define a two-dimension list at first and replace correct value to them. So I did:

```
# n is an integer and given by the question.
n = 3 
ans = [[None] * n] * n
# We got
# ans = [[None] * n] * n
```

It looks perfect. After my program ended, a list with three duplicated list was what I got.

## Reason

It's actually not surprising. The mutiplication operator __copy__ the first list in ans. We can see below code. Looking into the address of these three lists.

```
n = 3
ans = [[None] * n] * n
print(id(ans[0]))
print(id(ans[1]))s
print(id(ans[2]))
# We get
# 140595244466688
# 140595244466688
# 140595244466688
```

Obviously, all of three lists refer to the same address. So that we modify one of them and we will affect __ALL__.

```
ans[0][0] = 1
print(ans)
# [[1, None, None], [1, None, None], [1, None, None]]
```

## Solution

If you don't expect this behavior, you're able to try this way. It makes sure that you get three independent lists.

```
n = 3
ans = [ [None] * n for _ in range(n)]
ans[0][0] = 1
print(ans)
# We get
# [[1, None, None], [None, None, None], [None, None, None]]
```

## References

For more information about multiplication operator on list, like that you wonder why this behavior only occurs on list type. That's because the concept of [Mutable vs Immutable Objects in Python](https://medium.com/@meghamohan/mutable-and-immutable-side-of-python-c2145cf72747).

Others: [python * operator, creating list with default value](https://stackoverflow.com/questions/29306418/python-operator-creating-list-with-default-value)

** Happy hacking **