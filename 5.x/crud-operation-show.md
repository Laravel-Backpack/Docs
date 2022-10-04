# Show Operation

--

<a name="about"></a>
## About

This CRUD operation allows your admins to preview an entry. When enabled, it will add a "Preview" button in the ListEntries view, that points to a show page. By default, it will show all attributes for that model:

![Backpack CRUD Show Operation](https://backpackforlaravel.com/uploads/docs-4-0/operations/show.png)

In case your entity is translatable, it will show a multi-language dropdown, just like Edit.

<a name="how-it-works"></a>
## How it Works

The ```/entity-name/{id}/show``` route points to the ```show()``` method in your EntityCrudController, which shows all columns that have been set up using [column types](/docs/{{version}}/crud-columns), by showing a ```show.blade.php``` blade file. 

<a name="enabling"></a>
## How to Use

To enable this operation, you need to use the ```ShowOperation``` trait on your CrudController:

```php
<?php

namespace App\Http\Controllers\Admin;

use Backpack\CRUD\app\Http\Controllers\CrudController;

class ProductCrudController extends CrudController
{
    use \Backpack\CRUD\app\Http\Controllers\Operations\ShowOperation;
}
```

This will:
- make a Preview button appear inside the List view; 
- allow access to the show view;

By default, the operation tries to show all db columns in the database, but _remove_ any columns and buttons that it thinks you _wouldn't want_ shown. Which works great for simple Eloquent Models, it'll _just work_. But for more complex Models, it might be preferrable to define your own columns, using the same syntax you're using when defining the ListOperation.


<a name="configuring"></a>
## How to Configure

### setupShowOperation()

You can manually define columns inside the ```setupShowOperation()``` method - thereby stopping the default "guessing" and "removing" of columns - you'll start from a blank slate and be in complete control of what columns are shown. For example:

```php
    // if you just want to show the same columns as inside ListOperation
    protected function setupShowOperation()
    {
        $this->setupListOperation();
    }
```

But you can also do both - let Backpack guess columns, and do stuff before or after that guessing, by calling the `autoSetupShowOperation()` method wherever you want inside your `setupShowOperation()`:

```php
    // show whatever you want
    protected function setupShowOperation()
    {
        // MAYBE: do stuff before the autosetup
        
        // automatically add the columns
        $this->autoSetupShowOperation();
    
        // MAYBE: do stuff after the autosetup
        
        // for example, let's add some new columns
        $this->crud->addColumn([
            'name' => 'table',
            'label' => 'Table',
            'type' => 'table',
            'columns' => [
                'name'  => 'Name',
                'desc'  => 'Description',
                'price' => 'Price',
            ]
        ]);
        $this->crud->addColumn([
            'name' => 'fake_table',
            'label' => 'Fake Table',
            'type' => 'table',
            'columns' => [
                'name'  => 'Name',
                'desc'  => 'Description',
                'price' => 'Price',
            ],
        ]);
        
        // or maybe remove a column
        $this->crud->removeColumn('text');
    }
```

<a name="how-to-overwrite"></a>
## How to Overwrite

In case you need to modify the show logic in a meaningful way, you can create a ```show()``` method in your EntityCrudController. The route will then point to your method, instead of the one in the trait. For example:

```php
use \Backpack\CRUD\app\Http\Controllers\Operations\ShowOperation { show as traitShow; }

public function show($id)
{
    // custom logic before
    $content = $this->traitShow($id);
    // custom logic after
    return $content;
}
```
