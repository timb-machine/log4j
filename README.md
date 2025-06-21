# [log4j](https://blog.talosintelligence.com/2021/12/apache-log4j-rce-vulnerability.html)

![](https://img.shields.io/badge/last--updated-December%202021%20-green) ![](https://img.shields.io/badge/src-public-orange)

[Rolling 2 day view of updates from this repo](https://github.com/CiscoCXSecurity/log4j/compare/master@%7B2day%7D...master)

## Kick banning attacks at the WAF

* Block ```\$\{.+\}```

Note: Some of the public WAF regular expressions out there are probably vulnerable to ReDoS. If you want to play, https://regex101.com/r/KqGG3W/3 is a decent playground but you want to keep the number of steps as low as possible.

## Paths to check

### UNIX

* ```/opt```
* ```/usr/local```
* ```/home```

### OS X

(see also UNIX)

* ```/Applications```
* ```/Library```
* ```/Users/*/Applications```
* ```/Users/*/Library```

### Windows

* ```c:\Program Files```
* ```c:\Program Files (x86)```
* ```c:\Documents and Settings```
* ```c:\Users```

## Dirty checks

* ```find /path/to/check -iname "*log4j*"```
* ```grep -rq log4j /path/to/check && echo log4j matches```
* ```find /path/to/check \( -iname "*.?ar" -o -iname "*.zip" \) | while read line; do echo $line; jar tvf $line | grep -i log4j; done```
* ```find /path/to/check \( -iname "*.tar.bz2" -o -iname "*.tar.bz2" \) | while read line; do echo $line; tar tvf $line | grep -i log4j; done```

## YARA rules

Running the rules:

* ```yara -r yara/log4j.yara /path/to/check```

Example here:

* https://gist.github.com/timb-machine/d5ca718201ce294f1e744dbbcf4feaac

### Personal

* log4jball.yara - Hunts for references to Log4J balls
* log4jjavaclass.yara - Hunts for references to Log4J java in class form
* log4jjavasrc.yara - Hunts for references to Log4J java in source form
* log4jimport.yara - Hunts for references to Log4J imports
* log4jJndiLookup.yara - Hunts for references to Log4J JndiLookup

## Source code to check

* https://codesearch.debian.net/search?q=log4j&literal=1&perpkg=1
