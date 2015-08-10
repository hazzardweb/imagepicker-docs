# Save/Load - Database

This example will show to save and load an image from database using the some functions used in the previously examples.

Before continuing with this example make sure you have read the [Modal & Inline](modal-inline.md) example.

For this we need some functions to connect to database, insert, update and select. The script comes with a Database class that can be found in `server/Database`.

First you need to edit `server/Database/config.php` and set you database connection details. 
You also need a table where you insert the images (or you can use your own table), but for this example I'll use the included SQL table: `example_images.sql`.

Now in `upload_avatar.php` include the Database class and use it as shown below:


```php
<?php
session_start();

// Let's say that you grab the user id from the session
$user_id = $_SESSION['user_id'];

// Include ImgPicker.php class
require dirname(__FILE__) . '/ImgPicker.php';

// Include Database classs
require dirname(__FILE__) . '/Database/Database.php';

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
    'load' => function($instance) {
        global $user_id;        
        // Select the image for the current user
        $db = new Database;
        $results = $db->table('example_images')
                      ->where('user_id', $user_id)
                      ->limit(1)
                      ->get();
        if ($results) 
            return $results[0]->image;
        else 
            return false;
    },
    
    // Upload start callback
    'upload_start' => function($image, $instance) {
        global $user_id;
        // Name the temp image as $user_id
        $image->name = '~'.$user_id.'.'.$image->type;   
    },
    // Crop start callback
    'crop_start' => function($image, $instance) {
       global $user_id;
       // Change the name of the image
       $image->name = $user_id.'.'.$image->type;
    },
    // Crop complete callback
    'crop_complete' => function($image, $instance) {
        global $user_id;
        // Save the image to database
        $data = array(
           'user_id' => $user_id,
           'image' => $image->name
        );

        $db = new Database;
        // First check if the image exists
        $results = $db->table('example_images')
                      ->where('user_id', $user_id)
                      ->limit(1)
                      ->get();

        // If exists update, otherwise insert
        if ($results) 
            $db->table('example_images')
               ->where('user_id', $user_id)
               ->limit(1)
               ->update($data);
        else
            $db->table('example_images')->insert($data);
    }
);

// Create ImgPicker instance
new ImgPicker($options);
?>
```
