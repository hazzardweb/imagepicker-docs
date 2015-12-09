# Modal & Inline

- [Modal](#modal)
- [Inline](#inline)

These examples will use `server/upload_avatar.php` for uploading the images.

### Modal

The fist option is to have the ImagePicker as a modal. For this you have to define the modal html a button that will open the modal and of course the JavaScript. The button that will open the modal must have this attribute `data-ip-modal="#myModal"` and the modal must have `id="myModal"`.

```markup
<!-- CSS -->
<link rel="stylesheet" href="assets/css/bootstrap.css"> <!-- optional -->
<link rel="stylesheet" href="assets/css/imgpicker.css">

<!-- JavaScript -->
<script src="assets/js/jquery-1.11.0.min.js"></script>
<script src="assets/js/jquery.Jcrop.min.js"></script>
<script src="assets/js/jquery.imgpicker.js"></script> 
<!-- for production use jquery.imgpicker.min.js-->


<!-- Image -->
<img src="http://www.gravatar.com/avatar/0?d=mm&s=150" id="avatar" width="200" height="200">

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

<script>
$(function() {
    //Call .imgPicker()
    $('#myModal').imgPicker({
        url: 'server/upload_avatar.php',
        aspectRatio: 1,
        deleteComplete: function() {
            // Restore default avatar
            $('#avatar').attr('src', 'http://www.gravatar.com/avatar/0?d=mm&s=150');
            // Hide modal
            this.modal('hide');
        },
        cropSuccess: function(image) {
            // Set #avatar src
            $('#avatar').attr('src', image.versions.avatar.url);
            // Hide modal
            this.modal('hide');
        }
    });
});
</script>
```

The entire HTML is customizable BUT make sure you keep the classes that start with `ip-`.

## Inline

With the inline option you can display ImagePicker anywhere you want. For this you have to define a container with all the HTML and of course the JavaScript.

```markup
<!-- Container -->
<div id="myContainer">
    <div class="btn btn-primary ip-upload">
        Upload 
        <input type="file" name="file" class="ip-file">
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

<script>
$(function() {
    //Call .imgPicker()
    $('#myContainer').imgPicker({
        url: 'server/upload_avatar.php',
        aspectRatio: 1,
        deleteComplete: function() {
            $('#avatar').attr('src', 'http://www.gravatar.com/avatar/0?d=mm&s=150');
            this.modal('hide');
        },
        cropSuccess: function(image) {
            $('#avatar').attr('src', image.versions.avatar.url);
            this.modal('hide');
        }
    });
});
</script>
```

The entire HTML is customizable BUT make sure you keep the classes that start with `ip-`.
