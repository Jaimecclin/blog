# Python _curses.error: setupterm: could not find terminal on Docker-compose

### Date

October 31, 2020 

## What's curses?

> The curses library provides fairly basic functionality, providing the programmer with an abstraction of a display containing multiple non-overlapping windows of text. The contents of a window can be changed in various ways—adding text, erasing it, changing its appearance—and the curses library will figure out what control codes need to be sent to the terminal to produce the right output.

In short, it's a Python module you're able to utilize it to handle screen output and keyboard input.

## The problem

My program needed to be terminated in a sequence, so I used this module to catch a keyboard input rather than Ctrl-C. Yesterday I'd like to deploy the python code which using the curses module in __docker-compose__, but this error popped out. 

docker-compose:

```
version: "3.7"
services:
  app:
    image: ubuntu_python38:v2
    working_dir: /tmp
    volumes:
      - C:\Documents\Works\Python\TestFolder:/tmp
    command: python3 in_terminal.py
```

Error message:
```
Traceback (most recent call last):
    File "in_terminal.py", line 5, in <module>
        stdscr = curses.initscr()
```

## Solution

This error message presents that this program needs to acquire __the handle of terminal__. But no terminal was given in the docker-compose file. So that we can resolve it by rectifying the docker-compose.

```
version: "3.7"
services:
  app:
    image: ubuntu_python38:v2
    working_dir: /tmp
    volumes:
      - C:\Documents\Works\Python\TestFolder:/tmp
    stdin_open: true 
    tty: true        
    command: python3 in_terminal.py
```

## Reference
1. [Python3 official docs](https://docs.python.org/3/howto/curses.html)
2. https://stackoverflow.com/questions/36249744/interactive-shell-using-docker-compose