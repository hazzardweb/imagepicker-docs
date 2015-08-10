# Security Concerns

Image Picker validates the user input and makes sure that executable files are not allowed. But still there are some aspects you should be aware of.

When you are using the PHP [callback options](configuration.md#callback-options) make sure you check if that image is for the current user, or however may be the case for your app.

For example in the `delete` callback you have to return `true` or `false`. Before returning make sure you check if that image belongs to the current user. 

The same thing applies for `crop_start`, make sure the image belongs to the current user.

You can use the included Database class to make these checks. Example for the `delete` callback:

```php
// Assuming  that you have loaded the Database class:
'delete' => function($filename, $instance) {
    global $user_id;
    // Select the image for the current user
    $db = new Database;
    $results = $db->table('example_images')
              ->where('user_id', $user_id)
              ->where('image', $filename)
              ->limit(1)
              ->get();
    if ($results) 
       return true;
    else 
       return false;
},
```
