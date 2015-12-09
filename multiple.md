# Multiple Images

Load multiple images from database. You can upload one image at the time, but as many as you want.

First set up the PHP code (see `server/upload_multi.php`):

```php
<?php
session_start();

// Include Database classs
require  __DIR__.'/DB/Database.php';

Database::connect(require __DIR__.'/DB/config.php');

// Include ImgPicker class
require __DIR__ . '/ImgPicker.php';

// Let's say that you grab the user id from the session
$userId = $_SESSION['user_id'];

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
    'load' => function () use ($userId) {
        // Load all images for the current user
        $db = new Database;
        $results = $db->table('example_images')
                      ->where('user_id', $userId)
                      ->get();
        
        $images = array();
        foreach ($results as $result) {
            $images[] = $result->image;
        }
        return $images;
    },
    
    // Upload start callback
    'upload_start' => function ($image) use ($userId) {
        // Name the temp image as $userId
        $image->name = '~'.$userId.'.'.$image->type;   
    },
    // Crop start callback
    'crop_start' => function ($image) use ($userId) {
       // Change the name of the image
       $image->name = $userId.'.'.$image->type;
    },
    // Crop complete callback
    'crop_complete' => function ($image) use ($userId) {
        $data = array(
            'user_id' => 1, // Set a user
            'image' => $image->name
        );
              
        $db = new Database;
        $db->table('example_images')->insert($data);
    }
);

// Create ImgPicker instance
new ImgPicker($options);
?>
```


Now the HTML and JavaScript (see `example-multi.html`):

```markup
<!-- Container -->
<div id="myContainer">
    <div class="btn btn-primary ip-upload">
        Upload <input type="file" name="file" class="ip-file">
    </div>
    <button type="button" class="btn btn-primary ip-webcam">Webcam</button>
    <div class="alert ip-alert"></div>
    <div class="ip-info">
        To crop this image, drag a region below and then click "Save Image"
    </div>
    <div class="ip-preview"></div>
    <div class="ip-rotate">
        <button type="button" class="btn btn-default ip-rotate-ccw">
            <i class="icon-ccw"></i>
        </button>
        <button type="button" class="btn btn-default ip-rotate-cw">
            <i class="icon-cw"></i>
        </button>
    </div>
    <div class="ip-progress">
        <div class="text">Uploading</div>
        <div class="progress progress-striped active">
            <div class="progress-bar"></div>
        </div>
    </div>
    <div class="ip-actions">
        <button type="button" class="btn btn-success ip-save">Save Image</button>
        <button type="button" class="btn btn-primary ip-capture">Capture</button>
        <button type="button" class="btn btn-default ip-cancel">Cancel</button>
    </div>
</div>

<!-- Append the images to this -->
<br><ul class="images"></ul>

<script>
$(function() {
    // Function that will append an image
    var addImage = function(image) {
        $('.images').append('<li>'+image.name+'</li>');
    };
    
    $('#myContainer').imgPicker({
        url: 'server/upload_multi.php',
        aspectRatio: 1,
        loadComplete: function(images) {
            // Append all images
            for (var i = 0; i < images.length; i++) {
                addImage(images[i]);
            }
        },
        cropSuccess: function(image) {
            // Append cropped image
            addImage(image);
        }
    });
});
</script>
```
