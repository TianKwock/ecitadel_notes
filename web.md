# Web basics

Identify the stack: ``` ps aux | grep -Ei 'apache|httpd|nginx' ``` or ``` php -v ``` 

Lock down file permissions:
```
cd /var/www/html
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
chown -R www-data:www-data /var/www/html
```

Restrict methods: dependent on stack 

PHP configs:
```
display_errors Off
log_errors On
expose_php Off
disable_functions exec,passthru,shell_exec,system,popen,proc_open
```



