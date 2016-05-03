---
layout: post
title:  "Install ruby 2.3.0 con rvm"
date:   2016-04-21 00:37:19 -0300
categories: jekyll update
---

```
gem 'omniauth', '>= 1.0.0'
gem 'devise', git: 'git@github.com:plataformatec/devise.git'
gem 'devise_token_auth', git: 'git://github.com/lynndylanhurley/devise_token_auth.git'
gem 'devise', git: 'git@github.com:plataformatec/devise.git' solves the problem.
omniauth is devise_token_auth dependency too.
```