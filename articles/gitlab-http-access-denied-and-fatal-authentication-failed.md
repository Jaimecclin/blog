# Gitlab HTTP Basic: Access denied and fatal:Authentication failed

### Dates
11/7/2020

## Introduction

Today was the first day I got started to learn how to CI/CD on Gitlab. Everything was fine until I tried to push my first change. I'm a VsCode lover, and it provides a great user interface to do git actions. I clicked the push button, and Gitlab requested me to log in. Okay, but actually this was the first time to do it. So I used the correct account. Then the error message jumped out. And the logging box never appeared again.

## Error message

![Errro image](https://i.imgur.com/DuXSisL.png)

```
remote: HTTP Basic: Access denied
fatal: Authentication failed for 'https://gitlab.com/Jaimecclin/taiwanstock.git/'
```

## Solution 

It seems that the wrong account was already remembered in my OS. My OS is windows 10. The solution is searching for "Credential Manager" in your system.

![](https://i.imgur.com/UEJuvy0.png)


Select "Windows Credentials" and find out the Gitlab account. 

![](https://i.imgur.com/mGEITVa.png)


Finally, rectify your account and password.


---

If this helps you, please give me a clap. Thanks!

Happy debugging!