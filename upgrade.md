# Upgrade Guide

- [Upgrading To 1.3.2 From 1.3](upgrade.md#upgrading-to-132-from-13)
- [Upgrading To 1.3 From 1.2](upgrade.md#upgrading-to-13-from-12)

## Upgrading To 1.3.2 From 1.3

- Replace `assets/js/jquery.imgpicker.js`.

## Upgrading To 1.3 From 1.2

- Replace `assets/js/jquery.imgpicker.js`.
- Replace `server/ImgPicker.php`.
- Copy `server/DB` to `server/`.
- To include the database class now use: 
```php
require  __DIR__.'/DB/Database.php';
Database::connect(require __DIR__.'/DB/config.php');
```
- Check [git.hazzardweb.com](https://git.hazzardweb.com) for the complete list of changes.
