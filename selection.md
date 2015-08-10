# Set Default Crop Selection

This example will show how to set a default selection after the image was uploaded using the `uploadSuccess` callback and accesing the plugin options.

Before continuing with this example make sure you have read the [Modal & Inline](modal-inline.md) example.

Below is only the JavaScript code:

```javascript
<script>
$(function() {
    $('#myContainer').imgPicker({
        url: 'server/upload_avatar.php',
        aspectRatio: 1,
        uploadSuccess: function(image) {
            // Calculate the default selection for the cropper
            var select = (image.width > image.height) ? 
                [(image.width-image.height)/2, 0, image.height, image.height] : 
                [0, (image.height-image.width)/2, image.width, image.width];
            
            // Set the setSelect option
            this.options.setSelect = select;
        },
        cropSuccess: function(image) {
            $('#avatar').attr('src', image.versions.avatar.url);
            this.modal('hide');
        }
    });
});
</script>
```
