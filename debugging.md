# Debugging

If you get an error with this message: `Invalid response` that means the server is not sending a valid JSON fromat. This may be caused from a PHP error. Here is want you can do to see what's causing that (using Google Chrome):

<iframe src="https://www.screenr.com/embed/xqWN"></iframe>

If you get errors in the Console tab about the Origin request make sure in `server/upload_*.php` you accept the request from where you send it by adding:

```php
header('Access-Control-Allow-Origin: yourwebsite.com');
header('Access-Control-Allow-Origin: www.yourwebsite.com');
```

If you get server errors about the file size, make sure you change the settings for `upload_max_filesize` and `post_max_size` in [php.ini](http://www.php.net/manual/en/ini.core.php#ini.sect.file-uploads)
