# Python — Simple examples for mock module

This article will bring you some simple examples for learning the mock module in unit tests.

### Date

December 27, 2020 


## System environment

- OS: CentOs 8
- Python version: 3.6.8

## Motivation 

These days I was assigned a task to make the unit tests completed. To make the tests simple, I learnt to utilize __mock__ module in Python. I had heard about this module for a while but didn't have chance to understand it clearly. Now I regret that I didn't learn it early. (Yeah, it's my fault.)   

## What can mock module offer to you?

It's a very useful tool that to cooperate with the unittest module. Assumed that you knew how to write unit tests. We write unit tests for each module to make sure they work perfectly. But sometimes some modules rely on others. We don't want to get over the scope of the module we want to test. Or it's difficult to trigger the special case to complete out tests. __Mock__ can help you a lot.  [Download this code.](https://github.com/Jaimecclin/literate-lamp)

## Starting from the unit tests

Here we have a class Demo to be tested.

src/demo.py
```python
import datetime

class ValueError(Exception):
    pass

class SpecialError(Exception):
    pass

class Bias():
    
    @staticmethod
    def get_bias():
        return 5
        
class Demo:

    def __init__(self, num1, num2):
        self._num1 = num1
        self._num2 = num2

    @property
    def num1(self):
        return self._num1
    
    @property
    def num2(self):
        return self._num2

    def _special_number(self):
        ''' This might be a function using third-party methods '''
        return 1

    def sum(self):
        n = self._special_number()
        if n < 0:
            raise ValueError
        return self._num1 + self._num2 + n 
    
    def sum_bias(self):
        return self._num1 + self._num2 + Bias.get_bias()

    def append_datetime(self, string):
        return str(datetime.datetime.today()) + string
```

This is an unit test file.

tests/test_demo.py 
```python
import unittest
from unittest.mock import Mock
from src.demo import Demo

class TestDemo(unittest.TestCase):

    def test_basic(self):
        demo = Demo(1, 2)
        self.assertEqual(1, demo.num1)
        self.assertEqual(2, demo.num2)
        self.assertEqual(1, demo._special_number())
        self.assertEqual(4, demo.sum())
        self.assertEqual(8, demo.sum_bias())
```

Try it
```
$ python3 -m unittest tests/test_demo.py
```

## Add mock into tests

You can see we only have one test. If _special_number() gives a negative, sum() throws a exception. We can try this case without modifying the source code.

```python 
from unittest.mock import Mock, MagicMock, patch
from src.demo import Demo, ValueError, SpecialError

class TestDemo(unittest.TestCase):

    def test_raise_error(self):
        demo = Demo(1, 2)
        demo._special_number = MagicMock(return_value=-1)
        with self.assertRaises(ValueError):
            demo.sum()
        demo._special_number.assert_called_with()
```

The MagicMock object can replace the returning value of _special_number as -1. So that we can test the error cases easily. The below code snippet have the same effect, it just uses __with__ to do it.

```python
class TestDemo(unittest.TestCase):

    def test_raise_error_patch(self):    
        with patch.object(Demo, '_special_number', return_value=-1) as mocked_mathod:
            demo = Demo(1, 2)
            with self.assertRaises(ValueError):
                demo.sum()
        mocked_mathod.assert_called_once_with()
```

We can also make it raise an exception when the function is called. The assigned side_effect will be thrown out so it can be caught.

```python
class TestDemo(unittest.TestCase):

    def test_raise_error_side_effect(self):
        demo = Demo(1, 2)
        demo._special_number = MagicMock(side_effect=SpecialError)
        with self.assertRaises(SpecialError):
            demo.sum()
        demo._special_number.assert_called_with()
```

The real world is always complicated. None of modules works alone. However, I want the tests to be simple. We can mock those third-party library to generate our testing cases.

```python
class TestDemo(unittest.TestCase):

    def test_mock_sum_bias(self):
        demo = Demo(1, 2)
        self.assertEqual(8, demo.sum_bias())

    @patch('src.demo.Bias')
    def test_sum_bias(self, mocked_bias):
        mocked_bias.get_bias.return_value = 7
        demo = Demo(1, 2)
        self.assertEqual(10, demo.sum_bias())
```

The __patch__ decorator with arguments __src.demo.Bias__ would mock the Bias class until this function is ended. Therefore we can modify the returning value as we want.


In the Blow code snippet, we mock the datatime module. It shows the power of this module in writing tests. The datetime.today() gives a string “This is a mocked method” rather than the string of current time because we replace the function result.

```python
class TestDemo(unittest.TestCase):

    def test_datetime(self):
        demo = Demo(1, 2)
        self.assertTrue('abc' in demo.append_datetime('abc'))
    
    @patch('src.demo.datetime')
    def test_mock_datetime(self, mocked_datetime):
        mocked_datetime.datetime.today.return_value = 'This is a mocked method'
        demo = Demo(1, 2)
        self.assertTrue('mocked' in demo.append_datetime('abc'))
        mocked_datetime.datetime.today.assert_called_once()
```


Try to imagine that you have a module that needs to use the requests module to download files from the network. But you don’t want to waste time downloading real files. In this case, you could mock the requests module to simplify your testing.

Simple is better.