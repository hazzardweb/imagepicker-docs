# Autoload Image From Server

This example will show to autoload an image from server using the `loadComplete` function.

Before continuing with this example make sure you have read the [Modal & Inline](modal-inline.md) example.

First we start with the PHP:

```php
<?php
session_start();

// Let's say that you grab the user id from the session
$userId = $_SESSION['user_id'];

// Include ImgPicker.php class
require __DIR__ . '/ImgPicker.php';

// ImgPicker options
$options = array(
    // Upload directory path
    'upload_dir' => __DIR__ . '/../files/',
    // Upload directory url
    'upload_url' => 'files/',
    // Image versions
    'versions' => array(
        // Generate 200x200 square image for the avatar
        'avatar' => array(
            'crop' => true,
            'max_width' => 200,
            'max_height' => 200
        )
    ),
    // !!! Here you have to return the user image
    'load' => function () use ($userId) {
        // Make a query from database and return the filename:
        return '123.png';
    },
    // Upload start callback
    'upload_start' => function ($image) use ($userId) {
        // Name the temp image as $user_id
        $image->name = '~'.$user_id.'.'.$image->type;   
    },
    // Crop start callback
    'crop_start' => function ($image) use ($userId) {
       // Change the name of the image
       $image->name = $user_id.'.'.$image->type;
    },
    // Crop complete callback
    'crop_complete' => function ($image) use ($userId) {
        // Save the image to database
    }
);

// Create ImgPicker instance
new ImgPicker($options);
```

And the JavaScript code:

```javascript
<script>
$(function() {
    $('#myModal').imgPicker({
        url: 'server/upload_avatar.php',
        aspectRatio: 1,
        // We use the loadComplete to set the image
        loadComplete: function(image) {
            // Set #avatar image src
            $('#avatar').attr('src', image.versions.avatar.url);
            // Set the image for re-crop
            this.setImage(image);
        },
        cropSuccess: function(image) {
            $('#avatar').attr('src', image.versions.avatar.url);
            this.modal('hide');
        }
    });
});
</script>
```
