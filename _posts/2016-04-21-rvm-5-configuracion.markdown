---
layout: post
title:  "Instalar ruby 2.3.0 con rvm"
date:   2016-04-21 00:37:19 -0300
categories: ruby
---

Documentación oficial:

<b>https://rvm.io/rvm/install</b>

Para instalar rvm

```
\curl -sSL https://get.rvm.io | bash -s stable --ruby
```

Para ver que version de ruby tenemos instaldo:

```
➜  rvm list

rvm rubies

 * ruby-2.2.4 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
```

Para instalar la version ruby 2.3
```
➜ rvm install ruby-2.3.0
rvm install ruby-2.3.0
Warning, new version of rvm available '1.27.0', you are using older version '1.26.11'.
You can disable this warning with:    echo rvm_autoupdate_flag=0 >> ~/.rvmrc
You can enable  auto-update  with:    echo rvm_autoupdate_flag=2 >> ~/.rvmrc
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.11/x86_64/ruby-2.3.0.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
Updating certificates in '/usr/local/etc/openssl/cert.pem'.
Requirements installation successful.
Installing Ruby from source to: /Users/fbarrios/.rvm/rubies/ruby-2.3.0, this may take a while depending on your cpu(s)...
ruby-2.3.0 - #downloading ruby-2.3.0, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 13.5M  100 13.5M    0     0   116k      0  0:01:59  0:01:59 --:--:--  257k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.3.0 - #extracting ruby-2.3.0 to /Users/fbarrios/.rvm/src/ruby-2.3.0 - please wait
ruby-2.3.0 - #configuring - please wait
ruby-2.3.0 - #post-configuration - please wait
ruby-2.3.0 - #compiling - please wait
ruby-2.3.0 - #installing - please wait
ruby-2.3.0 - #making binaries executable - please wait
Installed rubygems 2.5.1 is newer than 2.4.8 provided with installed ruby, skipping installation, use --force to force installation.
ruby-2.3.0 - #gemset created /Users/fbarrios/.rvm/gems/ruby-2.3.0@global
ruby-2.3.0 - #importing gemset /Users/fbarrios/.rvm/gemsets/global.gems - please wait
ruby-2.3.0 - #generating global wrappers - please wait
ruby-2.3.0 - #gemset created /Users/fbarrios/.rvm/gems/ruby-2.3.0
ruby-2.3.0 - #importing gemsetfile /Users/fbarrios/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.3.0 - #generating default wrappers - please wait
ruby-2.3.0 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.3.0 - #complete
Ruby was built without documentation, to build it run: rvm docs generate-ri
```