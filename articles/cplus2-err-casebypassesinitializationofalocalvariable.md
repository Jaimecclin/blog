# C++ error “Case bypasses initialization of a local variable”
I came across this error when I tried to modify a complicated code this morning. I’d like to make sure it goes into eEvent4 condition, so I declared a string to record it in the log. And I got “Case bypasses initialization of a local variable”.
```C++=
switch (eEvent)
{
  case eEvent1:
  case eEvent2:
  case eEvent3:
  case eEvent4:
    string msg = "This is Event4";
    Trace.TraceSimple(msg);
  case eEvent5:
    break;
}
```
After wonderful google, we need to add curly brackets {} to solve it.
```
switch (eEvent)
{
  case eEvent1:
  case eEvent2:
  case eEvent3:
  case eEvent4:
  {
    string msg = "This is Event4";
    Trace.TraceSimple(msg);
  }
  case eEvent5:
    break;
}
```
Have fun!