# Python Multiprocessing Manager Error-‘ForkAwareLocal’ object has no attribute

** Technique record **

Python version: 3.6

Half-year ago I wrote a program using multiprocessing module to execute many machine learning tasks parallelly. One requirement is to dump real-time training results to the main console. Manager.Queue in multiprocessing module is used to communicate with the main process. At that time, there is no much time to do complete testing. Today I tried to write unit tests to validate it but I got this.

## Error message:

```
Traceback (most recent call last):
  File "/usr/lib/python3.6/multiprocessing/managers.py", line 749, in _callmethod
    conn = self._tls.connection
AttributeError: 'ForkAwareLocal' object has no attribute 'connection'
```

I think of it for a while and get a solution. I made this mistake before. So I need to record it. :(

## Solution

The root cause of this problem is the main thread is done so it crashes the connection between two processes. However, the subprocess is still running and it needs to throw stuff out via the queue. So keep the main thread alive is our solution.

In my production environment, the main thread won’t use process.join() because it has to handle other things. But now I am programming a unit test so as the main thread uses join command to wait until the subprocess is finished.

** Happy hacking **