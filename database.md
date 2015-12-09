# Save/Load - Database

This example will show to save and load an image from database using the some functions used in the previously examples.

Before continuing with this example make sure you have read the [Modal & Inline](modal-inline.md) example.

For this we need some functions to connect to database, insert, update and select. The script comes with a Database class that can be found in `server/DB`.

First you need to edit `server/DB/config.php` and set you database connection details. 
You also need a table where you insert the images (or you can use your own table), but for this example I'll use the included SQL table: `example_images.sql`.

Now in `upload_avatar.php` include the Database class and use it as shown below:


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
        // Select the image for the current user
        $db = new Database;
        $results = $db->table('example_images')
                      ->where('user_id', $userId)
                      ->limit(1)
                      ->get();

        if ($results) {
            return $results[0]->image;
        } else {
            return false;
        }
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
        // Save the image to database
        $data = array(
           'user_id' => $userId,
           'image' => $image->name
        );

        $db = new Database;
        // First check if the image exists
        $results = $db->table('example_images')
                      ->where('user_id', $userId)
                      ->limit(1)
                      ->get();

        // If exists update, otherwise insert
        if ($results) {
            $db->table('example_images')
               ->where('user_id', $userId)
               ->limit(1)
               ->update($data);
        } else {
            $db->table('example_images')->insert($data);
        }
    }
);

// Create ImgPicker instance
new ImgPicker($options);
```
