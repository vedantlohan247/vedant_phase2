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
