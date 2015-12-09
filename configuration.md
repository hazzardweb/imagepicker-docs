# PHP Configuration

- [Options](#options)
- [Callback Options](#callback-options)
- [Messages](#messages)

When you create a new instance of `ImgPicker` you can pass some options:

```php
<?php
$options = array(
   'upload_dir' => __DIR__ . '/../files/',
   'max_file_size' => 3000000 
)
new ImgPicker($options);
```

## Options

### upload_dir

Upload directory path

- Type: _string_
- Default: `server/files/`

### upload_url

Upload directory url

- Type: _string_
- Default: `full url to server/files/`

### accept_file_types

Accepted file types.

- Type: _string_
- Default: `png|jpg|jpeg|gif`

### mkdir_mode

- Directory mode.

- Type: _integer_
- Default: `0777`

### max_file_size

File max size restriction in bytes (e.g. 5000000 = 5MB).

- Type: _integer_
- Default: `null`

### min_file_size

File min size restriction in bytes (e.g. 1 = 1B).

- Type: _integer_
- Default: `1`

### max_width

Image max width restriction (e.g. 100 = 100 pixels).

- Type: _integer_
- Default: `null`

### max_height

Image max height restriction (e.g. 100 = 100 pixels).

- Type: _integer_
- Default: `null`

### min_width

Image min width restriction (e.g. 100 = 100 pixels).

- Type: _integer_
- Default: `1`

### min_height

Image min height restriction (e.g. 100 = 100 pixels).

- Type: _integer_
- Default: `1`

### versions

Image versions. Array with image versions to be created. Each element is another array which has can have other options.

- Type: _array_
- Default: `n/a`

```php
'versions' => array(
    // This will create 2 image versions: the original one and a 200x200 one
    'avatar' => array(
        // Other upload directory path (Default: main upload_dir)
        'upload_dir' => __DIR__ . '/../files/avatar/',
        // Other upload directory url (Default: main upload_url)
        'upload_url' => 'files/avatar/',
        // Create square image (Default: false)
        'crop' => true,
        // Max width (Default: n/a)
        'max_width' => 200, 
        // Max height (Default: n/a)
        'max_height' => 200
    ),
)
```

If you want to have only one image create a version with an empty name:

```php
'versions' => array(
    // This will create only one image
    '' => array(
        // The same options as above can be used here
    ),
)
```

## Callback Options

The callbacks are functions that will be called once an action is completed. In all of these callbacks you can retrive data sent from the Client Side using `$_POST['data']`` or `$_GET['data']`` for the load callback. To send data from the Client Side check out [Options > data](options.md#data)

### load

Called when autoloading image.

```php
 /**
 *  @return string|array
 */
'load' => function () {
    // You can make a query from your database and return the image:
    // return '123.jpg'; |  return array('1.jpg', '2.jpg');
},
```

### delete

Called when trying to delete image.

```php
 /**
 *  @param  stdClass $filename
 *  @return boolean
 */
'delete' => function ($filename) {
    // Wether to allow file deletion
    return true;
},
```

### upload_start

Called before the upload starts.

```php
 /**
 *  @param  stdClass $image
 *  @return void
 */
'upload_start' => function ($image) {
},
```

### upload_complete

Called after upload.

```php
 /**
 *  @param  stdClass $image
 *  @return void
 */
'upload_complete' => function ($image) {
},
```

### crop_start

Called before the crop starts.

```php
 /**
 *  @param  stdClass $image
 *  @return void
 */
'crop_start' => function ($image) {
},
```

### crop_complete

Called after cropping.

```php
 /**
 *  @param  stdClass $image
 *  @return void
 */
'crop_complete' => function ($image) {
},
```

In all callbacks the `$image` parameter is an `stdClass` object and has the following proprieties:

- `name` _string_
- `type` _string_
- `size` _integer_ (available only in _upload_start_)
- `path` _string_
- `url` _string_
- `width` _integer_
- `height` _integer_
- `versions` _array_ (available only in _upload_complete_ and _crop_complete_)

You can access them like this: `$image->name`, `$image->path`.

### Messages

If you wish to change the messages, pass an array with the messages:

```php
<?php

$options = array();

$messages = array(
    1 => 'The uploaded file exceeds the upload_max_filesize directive in php.ini',
    2 => 'The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form',
    3 => 'The uploaded file was only partially uploaded',
    4 => 'No file was uploaded',
    6 => 'Missing a temporary folder',
    7 => 'Failed to write file to disk',
    8 => 'A PHP extension stopped the file upload',
    'gd' => 'PHP GD library is NOT installed on your web server',
    'post_max_size' => 'The uploaded file exceeds the post_max_size directive in php.ini',
    'max_file_size' => 'File is too big',
    'min_file_size' => 'File is too small',
    'accept_file_types' => 'Filetype not allowed',
    'max_width'  => 'Image exceeds maximum width of ',
    'min_width'  => 'Image requires a minimum width of ',
    'max_height' => 'Image exceeds maximum height of ',
    'min_height' => 'Image requires a minimum height of ',
    'upload_failed' => 'Failed to upload the file',
    'move_failed' => 'Failed to upload the file',
    'invalid_image' => 'Invalid image',
    'image_resize' => 'Failed to resize image',
    'not_exists' => 'Failed to load the image'
); 
new ImgPicker($options, $messages);
```
