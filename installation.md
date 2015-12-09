# Installation

- [Install Image Picker](#install-image-picker)
- [Quick Configuration](#quick-configuration)
- [Server Requirements](#server-requirements)
- [Browser Support](#browser-support)

## Install Image Picker

- Extract and copy the files from the archive you have downloaded from CodeCanyon to your server or local server.
- Make sure that the `files` directory has `755` (or `777`) permission. 
- Now you can run the script in your browser. See the included examples.

## Quick Configuration

Image Picker has two sets of options: the jQuery plugin options (see [Options](options.md) section) and the PHP class options (see [Configuration](configuration.md) section). The included examples have some options set, but you can change them however you want.

This is how the jQuery plugin options look like:

```javascript
$('#avatarModal').imgPicker({
    url: 'server/upload_avatar.php',
    aspectRatio: 1,
    cropSuccess: function(image) {
        $('#avatar').attr('src', image.versions.avatar.url);
        this.modal('hide');
    }
});
```

This is how the PHP class options look like:

```php
$options = array(
    'upload_dir' => __DIR__ . '/../files/',
    'upload_url' => 'files/',
    'accept_file_types' => 'png|jpg|jpeg|gif',
    'versions' => array(
        'avatar' => array(
            'crop' => true,
            'max_width' => 200,
            'max_height' => 200
        )
    )
);

new ImgPicker($options);
```

## Server Requirements 

Image Picker requires PHP 5.3+ with [GD](http://php.net/manual/en/book.image.php).

## Browser Support

Image Picker works on all major browsers (including mobile) with some exceptions:

- Chrome
- Firefox
- Opera
- Safari 5+
- IE8+

Starting with version 47, Chrome requires a secure connection (SSL) in order to use the Webcam. Read more [here](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins).

In Safari 5, IE8 and IE9 only the upload works due to no [XHR](http://caniuse.com/#search=XMLHttpRequest) support.
