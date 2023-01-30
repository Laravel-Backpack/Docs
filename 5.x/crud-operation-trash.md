# Trash Operation <span class="badge badge-info">PRO</span>

--

<a name="about"></a>
## About

This CRUD operation allows your admins to soft delete, restore and permanently delete entries from the table.

<a name="requirements"></a>
## Requirements

1. This is a <span class="badge badge-info">PRO</span> operation. It requires that you have [purchased access to `backpack/pro`](https://backpackforlaravel.com/products/pro-for-unlimited-projects).

2. In addition, it needs that your Model uses Laravel's `SoftDeletes` trait. To have your Model use SoftDeles, you should:
- generate a migration to add the `deleted_at` column to your table, eg. `php artisan make:migration add_soft_deletes_to_products --table=products`;
- inside that file's `Schema::table()` closure, add `$table->softDeletes();`
- run `php artisan migrate`
- add `use SoftDeletes` on the corresponding model (and import that class namespace);  

<a name="trash-a-single-item"></a>
## Trash a Single Item <span class="badge badge-info">PRO</span>

<a name="how-it-works"></a>
### How it Works
- **Trash**:
Using AJAX, a DELETE request is performed towards ```/entity-name/{id}/trash```, which points to the ```trash()``` method in your EntityCrudController.

- **Restore**:
Using AJAX, a PUT request is performed towards ```/entity-name/{id}/restore```, which points to the ```restore()``` method in your EntityCrudController.

- **Delete**:
Using AJAX, a DELETE request is performed towards ```/entity-name/{id}/delete-permanently```, which points to the ```deletePermanently()``` method in your EntityCrudController.

<a name="enabling"></a>
### How to Use

To enable it, you need to ```use \Backpack\Pro\Http\Controllers\Operations\TrashOperation;``` on your EntityCrudController. For example:

```php
<?php

namespace App\Http\Controllers\Admin;

use Backpack\CRUD\app\Http\Controllers\CrudController;

class ProductCrudController extends CrudController
{
    use \Backpack\Pro\Http\Controllers\Operations\TrashOperation;
}
```

This will make a Trash button and Trashed filter show up in the list view, and will enable the routes and functionality needed for the operation.

<a name="how-to-overwrite"></a>
### How to Overwrite

In case you need to change how this operation works, just create ```trash()```, ```restore()```,```deletePermanently()``` methods in your EntityCrudController:

```php
use \Backpack\Pro\Http\Controllers\Operations\TrashOperation { trash as traitTrash; }

public function deletePermanently($id)
{
    $this->crud->hasAccessOrFail('delete_permanently');

    // your custom code here
}

public function trash($id)
{
    $this->crud->hasAccessOrFail('trash');

    // your custom code here
}

public function restore($id)
{
    $this->crud->hasAccessOrFail('restore');

    // your custom code here
}
```

You can also override the buttons by creating a file with the same name inside your ```resources/views/vendor/backpack/crud/buttons/```. You can easily publish the buttons there to make changes using:

```zsh
php artisan backpack:button --from=trash

php artisan backpack:button --from=restore

php artisan backpack:button --from=delete_permanently
```

<a name="trash-multiple-items-bulk-trash"></a>
## Trash Multiple Items (Bulk Trash) <span class="badge badge-info">PRO</span>

In addition to the button for each entry, <span class="badge badge-info">PRO</span> developers can show checkboxes next to each element, to allow their admin to trash, restore & delete multiple entries at once.


<a name="how-it-works"></a>
### How it Works

### How it Works
- **Trash**:
Using AJAX, a DELETE request is performed towards ```/entity-name/{id}/bulk-trash```, which points to the ```bulkTrash()``` method in your EntityCrudController.

- **Restore**:
Using AJAX, a PUT request is performed towards ```/entity-name/{id}/bulk-restore```, which points to the ```bulkRestore()``` method in your EntityCrudController.

- **Delete**:
Using AJAX, a DELETE request is performed towards ```/entity-name/{id}/bulk-delete```, which points to the ```bulkDelete()``` method in your EntityCrudController.

<a name="enabling"></a>
### How to Use

You need to ```use \Backpack\Pro\Http\Controllers\Operations\BulkTrashOperation;``` on your EntityCrudController.

<a name="how-to-overwrite"></a>
### How to Overwrite

In case you need to change how this operation works, just create a ```bulkTrash()``` method in your EntityCrudController:

```php
use \Backpack\Pro\Http\Controllers\Operations\BulkTrashOperation { bulkTrash as traitBulkTrash; }

public function bulkTrash($id)
{
    $this->crud->hasAccessOrFail('bulkTrash');

    // your custom code here
}

public function bulkRestore($id)
{
    $this->crud->hasAccessOrFail('bulkRestore');

    // your custom code here
}

public function bulkDelete($id)
{
    $this->crud->hasAccessOrFail('bulkDelete');

    // your custom code here
}
```

You can also override the buttons by creating a file with the same name inside your ```resources/views/vendor/backpack/crud/buttons/```. You can easily publish the buttons there to make changes using:

```zsh
php artisan backpack:button --from=trash

php artisan backpack:button --from=restore

php artisan backpack:button --from=delete_permanently
```