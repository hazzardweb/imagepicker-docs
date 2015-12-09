# jQuery Plugin Options

- [Options](#options)
- [Callback Options](#callback-options)
- [Modal Events](#modal-events)

On the client side Image Picker is implemented as a jQuery plugin so you should be familiar with this one.

## Options

The options are set like this:

```javascript
$('#myModal').imgPicker({
    url: 'server/upload_avatar.php',
    aspectRatio: 1,
    // other options
});
```

### url

A string containing the URL to which the request is sent. 

- Type: _string_
- Default: `server/upload.php`

### dropZone

The drop target jQuery object. If set, when you drop the file in the target elementthe file will be uploaded.

- Type: `jQuery Object`
- Default: modal/inline containers

### crop

Whether crop is enabled or not.

- Type: _boolean_
- Default: `true`

### aspectRatio

Aspect ratio of width/height (e.g. 1 for square).
- Type: _decimal_
- Default: `n/a`


### minSize

Minimum width/height, 0 for unbounded dimension.

- Type: _array [ w, h ]_
- Default: `n/a`


### maxSize

Maximum width/height, use 0 for unbounded dimension

- Type: _array [ w, h ]_
- Default: `n/a`


### setSelect

Set an initial selection area.

- Type: _array [ x1, y1, x2, y2 ]_
- Default: `n/a`

### swf

Flash swf url.

- Type: _string_
- Default: `assets/webcam.swf`


### swfSize

Flash swf width/height.

- Type: _array [ w, h ]_
- Default: `[470, 350]`

### data

Custom data to be passed to server.

- Type: _object_ / _function_
- Default: `{}`

As object: `data: { post_id: 123, category_id: 321 }`

Or as function:

```javascript
data: function() {
    return {
        post_id: $('#my-post-input').val(),
        category_id: 321,
    };
}
```

On the server side you can grab the custom data using the `$_REQUEST['data']` array. <br>

```php
$postId = $_REQUEST['data']['post_id'];
```

### messages

Object of messages.

- Type: _object_
- Default: see `assets/js/jquery.imgpicker.js`

## Callback Options

The callbacks are functions that will be called once an action is completed. In these functions you can use the `this` keyword to access the current `imgPicker` instance. <br>
For example to access the options object you use `this.options`

```javascript
$('#myModal').imgPicker({
    deleteComplete: function() {
        alert('Image deleted');
    },
    cropSuccess: function(image) {
        this.modal('hide'); // Will call the modal method
    }
});
```

### cropSuccess(image)

Called after the crop was made. The __image__ parameter in an object will all of the image properties. To access a sub proprietary use `image.name` / `image.versions.avatar.url`. Example of how the __image__ object may look like:

```javascript
{
    name: "avatar.jpg",
    type: "jpg",
    height: 1200,
    width: 1920,
    url: "files/avatar.jpg",
    versions: {
       avatar: {
           height: 200,
           width: 200,
           url: "files/avatar-avatar.jpg"
       }
    }
}
```

### uploadSuccess(image)

Called after the image was uploaded. The image parameter is the same as in the [cropSuccess](#cropsuccessimage) callback.

### uploadProgress(percentage)

Called each time the upload is progressing. The __percentage__ parameter ranges from 0 to 100. By using this, the default function will be overwrite.

### uploadProgressComplete(callback)

Called when the upload progress is completed. Make sure you call `callback()` at the end of this function. By using this, the default function will be overwrite.

### loadComplete(image)

Called after the image was loaded from server. The __image__ parameter is the same as in the [cropSuccess](#cropsuccessimage) callback.

### deleteComplete()

Called after the image was deleted.

### alert(message, messageType)

Called each time there are error/loading/success messages. By using this, the default function will be overwrite.

## Modal Events

```javascript
// Bind events
$('#myModal').imgPicker({
    // ... options
}).on('shown.ip.modal', function() {
    // Modal shown
}).on('hidden.ip.modal', function() {
    // Modal hidden
});

// Triggers
$('#myModal').trigger('show.ip.modal'); // Show modal
$('#myModal').trigger('hide.ip.modal'); // Hide modal
```
