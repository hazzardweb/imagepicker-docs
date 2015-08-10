# Usage

- [Server Side](#server-side)
- [Client Side](#client-side)

## Server Side

On the server side you have to to set up the the PHP file where the requests are sent. These types of files are found in the `server` directory. 

This is how this file should look like:

```php
<?php
session_start();

// Let's say that you grab the user id from the session
$user_id = $_SESSION['user_id'];

// Include ImgPicker.php class
require dirname(__FILE__) . '/ImgPicker.php';

// ImgPicker options
$options = array(
    // Upload directory path
    'upload_dir' => dirname(__FILE__) . '/../files/',
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
    // Upload start callback
    'upload_start' => function ($image, $instance) {
        global $user_id;
        // Name the temp image as $user_id
        $image->name = '~'.$user_id.'.'.$image->type; 
    },
    // Crop start callback
    'crop_start' => function ($image, $instance) {
       global $user_id;
       // Change the name of the image
       $image->name = $user_id.'.'.$image->type;
    },
    // Crop complete callback
    'crop_complete' => function ($image, $instance) {
        // Save the image to database
    }
);

// Create ImgPicker instance
new ImgPicker($options);
```

## Client Side

First you have to include the required files:

```markup
<!-- CSS -->
<link rel="stylesheet" href="assets/css/bootstrap.css">
<link rel="stylesheet" href="assets/css/imgpicker.css">

<!-- JavaScript -->
<script src="assets/js/jquery-1.11.0.min.js"></script>
<script src="assets/js/jquery.Jcrop.min.js"></script>
<script src="assets/js/jquery.imgpicker.js"></script> <!-- for production use jquery.imgpicker.min.js-->
```

`bootstrap.css` is a trimmed version of Bootstrap, has only buttons, alert and progress bar. So even if you don't want to include Bootstrap in your website, this small file will not affect your website css.

The second step is to add the html. Here you have two options Modal and Inline. This is for the modal one:

```markup
<!-- Button trigger modal -->
<button type="button" data-ip-modal="#myModal">Launch modal</button>

<!-- Modal -->
<div class="ip-modal" id="myModal">
  <div class="ip-modal-dialog">
    <div class="ip-modal-content">
      <div class="ip-modal-header">
        <a class="ip-close" title="Close">&times;</a>
        <h4 class="ip-modal-title">Change avatar</h4>
      </div>
      <div class="ip-modal-body">
        <div class="btn btn-primary ip-upload">
            Upload <input type="file" name="file" class="ip-file">
        </div>
        <button type="button" class="btn btn-primary ip-webcam">Webcam</button>
        <button type="button" class="btn btn-info ip-edit">Edit</button>
        <button type="button" class="btn btn-danger ip-delete">Delete</button>
        <div class="alert ip-alert"></div>
        <div class="ip-info">
            To crop this image, drag a region below and then click "Save Image"
        </div>
       <div class="ip-preview"></div>
        <div class="ip-rotate">
          <button type="button" class="btn btn-default ip-rotate-ccw" >
            <i class="icon-ccw"></i>
          </button>
          <button type="button" class="btn btn-default ip-rotate-cw" >
            <i class="icon-cw"></i>
          </button>
        </div>
        <div class="ip-progress">
          <div class="text">Uploading</div>
          <div class="progress progress-striped active">
            <div class="progress-bar"></div>
          </div>
        </div>
      </div>
      <div class="ip-modal-footer">
        <div class="ip-actions">
          <button type="button" class="btn btn-success ip-save">Save Image</button>
          <button type="button" class="btn btn-primary ip-capture">Capture</button>
          <button type="button" class="btn btn-default ip-cancel">Cancel</button>
        </div>
        <button type="button" class="btn btn-default ip-close">Close</button>
      </div>
    </div>
  </div>
</div>
```

All you have to remember is that you have to add `data-ip-modal="#myModal"` to the button that triggers the modal and the modal must have `id="myModal"`. 
You can customize this however you want but remember that all HTML classes starting with ip- are required.

And finaly in JavaScript call the `.imgPicker()` jQuery method with `#myModal` as selector:


```javascript
<script>
$(function() {
    // We use this so the image won't be cached
    var time = function(){return'?'+new Date().getTime()};

    $('#myModal').imgPicker({
        url: 'server/upload_avatar.php',
        aspectRatio: 1, // Crop aspect ratio
        // Delete callback
        deleteComplete: function() {
            $('#avatar').attr('src', 'assets/img/default-avatar.png');
            this.modal('hide');
        },
        // Crop success callback
        cropSuccess: function(image) {
            $('#avatar').attr('src', image.versions.avatar.url + time());
            this.modal('hide');
        }
    });
});
</script>
```

You should also check the included examples that show more example of using Image Picker.
