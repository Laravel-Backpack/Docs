# Uploaders

---

<a name="upload-about"></a>
## About

Uploading and managing files is a common task in Admin Panels. Starting with Backpack v6, you can fully setup your upload fields in your field definition. No more mutators, manual validation of input, or custom code to handle the files.

<a name="upload-how-it-works"></a>
## How it works

When adding an upload field (`upload`, `upload_multiple`, `image` or `dropzone`) to your operations, you can now tell Backpack that you want it to upload those files to a specific disk, path and customize some additional things. How? Using `withFiles()`:
    
```php
CRUD::field('avatar')->type('upload')->withFiles();
```
> **IMPORTANT**: make sure you've linked the `storage` folder to your `public` folder. You can do that by running `php artisan storage:link` in your terminal.

That's it. Backpack will now handle the upload, storage and deletion of the files for you. By default it will use `public` disk, and will delete the files when the entry is deleted.

<a name="upload-configuration"></a>
## Configuring the uploads

The `withFiles()` method accepts an array of options that you can use to customize the upload. 

```php
CRUD::field('avatar')
    ->type('upload')
    ->withFiles([
        'disk' => 'public', // the disk where file will be stored
        'path' => 'uploads', // the path inside the disk where file will be stored
]);
```
**Note**: If you've defined `disk` or `prefix` on the field, you no longer need to define `path` within `withFiles()` - it will pick those up. Make sure you are not defining both.


##### Configuration options

- **`disk`** - default: **`public`**
The disk where the file will be stored. You can use any disk defined in your `config/filesystems.php` file.
- **`path`** - default: **`/`**
The path inside the disk where the file will be stored. It maps to `prefix` in field definition.
- **`deleteWhenEntryIsDeleted`** - default: **`true`**
The files will be deleted when the entry is deleted. Please take into consideration that `soft deleted models` don't delete the files.
- **`temporaryUrl`** - default: **`false`**
Some cloud disks like `s3` support the usage of temporary urls for display. Set this option to true if you want to use them.
- **`temporaryUrlExpirationTime`** - default: **`1`**
When `temporaryUrl` is set to `true`, this configures the amount of time in minutes the temporary url will be valid for.
- **`uploader`** - default: **null**
This allows you to overwrite or set the uploader class for this field. You can use any class that implements `UploaderInterface`.
- **`fileNamer`** - default: **null**
It accepts a `FileNameGeneratorInterface` instance or a closure. As the name implies, this will be used to generate the file name. Read more about in the [Naming uploaded files](#upload-name-files) section.

<a name="upload-name-files"></a>
#### Naming uploaded files

Backpack implements a default naming strategy for uploaded files that we have found out to work well in most scenarios. 

For `upload`, `upload_multiple` and `dropzone` fields, the file name will be the original file name slugged and with a random 4 character string appended to it. This is to avoid name collisions. Eg: `my file.pdf` becomes `my-file-aY5x.pdf`.

For `image` it will generate a unique name for the file, and will keep the original extension. Eg: `my file.jpg` becomes `5f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c.jpg`.

You can customize the naming strategy by creating a class that implements `FileNameGeneratorInterface` and pass it to the upload configuration (the default used by Backpack).

```php
CRUD::field('avatar')->type('upload')->withFiles([
        'fileNamer' => \Backpack\CRUD\app\Library\Uploaders\Support\FileNameGenerator::class,
]);

// alternativelly you can pass a closure:
->withFiles([
    'fileNamer' => function($file, $uploader) { return 'the_file_name.png'; },
])
```
<a name="uploads-in-subfields"></a>
#### Subfields

You can also use uploaders in subfields. The configuration is the same as for regular fields, just use the same `withFiles` key and pass it `true` if no further configuration is required.

```php
// subfields array
[
    [
        'name' => 'avatar',
        'type' => 'upload', 
        'withFiles' => true 
    ],
    [
        'name' => 'attachments',
        'type' => 'upload_multiple', 
        'withFiles' => [
            'path' => 'attachments',
        ],
    ],
]
```