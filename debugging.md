# Debugging

If you get an error with this message: `Invalid response` that means the server is not sending a valid JSON fromat. This may be caused from a PHP error.

You can use the browser dev tools to see the actual errors and response from server.

In Google Chrome, right-click and select __Inspect__, or use the keyboard shortcut: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd> (or <kbd>Cmd</kbd>+<kbd>Opt</kbd>+<kbd>I</kbd> on Mac).

Navigate to the __Console__ tab to see potential JavaScript errors and the __Network__ tab to see the HTTP requests (make sure _Record Network_ Log is on).

If you get errors in the Console tab about the Origin request make sure in `server/upload_*.php` you accept the request from where you send it by adding:

```php
header('Access-Control-Allow-Origin: yourwebsite.com');
header('Access-Control-Allow-Origin: www.yourwebsite.com');
```

If you get server errors about the file size, make sure you change the settings for `upload_max_filesize` and `post_max_size` in [php.ini](http://www.php.net/manual/en/ini.core.php#ini.sect.file-uploads)
