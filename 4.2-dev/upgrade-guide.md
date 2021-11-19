# Upgrade Guide

---

This will guide you to upgrade from Backpack 4.1 to 4.2. The steps are color-coded by the probability that you will need it for your application: <span class="badge badge-info text-white" style="text-decoration: none;">High</span>, <span class="badge badge-warning text-white" style="text-decoration: none;">Medium</span> and <span class="badge badge-secondary-soft" style="text-decoration: none;">Low</span>.

<a name="requirements"></a>
## Requirements

Please make sure your project respects the requirements below, before you start the upgrade process. You can check with ```php artisan backpack:version```:

- PHP 8.x, 7.4.x or 7.3.x
- Laravel 8.x
- Backpack\CRUD 4.1.x
- around 10 minutes for most projects, on top of the Laravel upgrade

**If you're running Backpack version 3.x or 4.0, please follow ALL the minor upgrade guides first, to incrementally get to use Backpack 4.1**. Test that your app works well with each version, after each upgrade. Only _afterwards_ can you follow this guide, to upgrade from 4.0 to 4.1. Previous upgrade guides:
- [upgrade from 4.0 to 4.1](https://backpackforlaravel.com/docs/4.1/upgrade-guide);
- [upgrade from 3.6 to 4.0](https://backpackforlaravel.com/docs/4.0/upgrade-guide); this is a major upgrade, and requires a v4 license code; if you've purchased a Backpack v3 license, but don't have a Backpack v4 license yet, [read the 4.0 Release Notes here](/docs/4.0/release-notes#backpack-v3-buyers).
- [upgrade from 3.5 to 3.6](https://backpackforlaravel.com/docs/3.6/upgrade-guide);
- [upgrade from 3.4 to 3.5](https://backpackforlaravel.com/docs/3.5/upgrade-guide);
- [upgrade from 3.3 to 3.4](https://backpackforlaravel.com/docs/3.4/upgrade-guide);

<a name="upgraade-steps"></a>
## Upgrade Steps


<a name="step-0" href="#step-0" class="badge badge-info" style="text-decoration: none;">Step 0.</a> **[Upgrade to Laravel 8](https://laravel.com/docs/8.x/upgrade)**, then test your app is working fine, then continue with "Step 1" below, to also upgrade Backpack to 4.2.

Note: If you're scared or lazy, for just $9 you can purchase a [Laravel Shift](https://laravelshift.com/?ref=backpack), to automatically upgrade to Laravel 8, and make sure you haven't missed a step.

<a name="composer"></a>
### Composer

<a name="step-1" href="#step-1" class="badge badge-info text-white" style="text-decoration: none;">Step 1.</a> Update your ```composer.json``` file to require ```"backpack/crud": "4.2.*"```

<a name="step-2" href="#step-2" class="badge badge-warning text-white" style="text-decoration: none;">Step 2.</a> If you have a lot of Backpack add-ons installed (and their dependencies), here are their latest versions, that support Backpack 4.2, you can copy-paste the versions of the packages you're using:

// TODO

```
        "backpack/logmanager": "^4.0.0",
        "backpack/settings": "^3.0.0",
        "backpack/pagemanager": "^3.0.0",
        "backpack/menucrud": "^2.0.0",
        "backpack/newscrud": "^4.0.0",
        "backpack/permissionmanager": "^6.0.0",
        "backpack/backupmanager": "^2.0.0",
        "spatie/laravel-translatable": "^4.0",
        "backpack/langfilemanager": "^3.0.0",

        /* and in require-dev */

        "backpack/generators": "^3.0",
        "laracasts/generators": "^1.0"
```

<a name="step-3" href="#step-3" class="badge badge-info text-white" style="text-decoration: none;">Step 3.</a> Run ```composer update``` in the command line.

<a name="models"></a>
### Models

No changes needed. // TODO

<a name="routes"></a>
### Routes

No changes needed.  // TODO

<a name="config"></a>
### Config

<a name="step-12" href="#step-12" class="badge badge-warning text-white" style="text-decoration: none;">Step 13.</a> This Backpack version also throttles password request attempts, so that a malicious actor cannot use the "reset password" functionality to send unsolicited emails. We recommend you add this configuration entry to your `config/backpack/base.php` and customize if necessary. See [#3862](https://github.com/Laravel-Backpack/CRUD/pull/3862) for details.

```php
// TODO: code that needs adding
```

<a name="controllers"></a>
### CrudControllers

<a name="step-x" href="#step-x" class="badge badge-danger text-white" style="text-decoration: none;">Step x.</a> We have removed the **`simplemde` field**, since it the JS library hasn't received any updates since 2016. However, there is a drop-in replacement called `easymde` which is well maintained. If you're using the `simplemde` field anywhere in your project, please use `easymde` instead. A simple find & replace should do.

<a name="step-y" href="#step-y" class="badge badge-warning text-white" style="text-decoration: none;">Step y.</a> If you're using the **Reorder operation**, but have overriden some of its functionality, please take note that the operation itself and its blade file has had a small change, so that the information is now passed as JSON. Take a look at the [changes here](https://github.com/Laravel-Backpack/CRUD/pull/3808/files) and do them in your own custom code too.

<a href="assets"></a>
### CSS & JS Assets

<a name="step-13" href="#step-13" class="badge badge-info text-white" style="text-decoration: none;">Step 13.</a> We've updated most CSS & JS dependencies to their latest versions. There are two ways to publish the latest styles and scripts for these dependencies:
- (A) If you have NOT touched you ```public/packages``` folder, or placed anything custom inside it:
        - delete the ```public/packages``` directory and all its contents;
        - run ```php artisan vendor:publish --provider="Backpack\CRUD\BackpackServiceProvider" --tag=public```
        - if you use elFinder, also delete ```resources/views/vendor/elfinder``` and run ```php artisan backpack:filemanager:install```
- (B) Run ```php artisan vendor:publish --provider="Backpack\CRUD\BackpackServiceProvider" --tag=public --force```. Please note this will overwrite anything that's already there. This B solution has a downside: unused files are not removed. A few files Backpack no longer uses will still be in your ```public/packages``` folder, even though they're no longer used.



<a name="views"></a>
### Views

No changes needed.  // TODO

<a name="security"></a>
### Security

<a name="step-17" href="#step-18" class="badge badge-danger text-white" style="text-decoration: none;">Step 17.</a> By default, all columns except `custom_html` and `markdown` now escape the output (they echo using `{{ }}` instead of `{!! !!}`). This was done to increase _default security_. If you've been sloppy about _input_ sanitization, Backpack at least tries to protect your _output_. If a malicious user entered that value, any string from the database might contain malicious JS code... and you probably don't want your admin exposed to it.
- If you've been showing HTML using the `array`, `array_count`, `closure`, `model_function`, `model_function_attribute`, `relationship_count` or `textarea` columns, please [read more about this](/docs/{{version}}/crud-columns#escape-column-output). If you decide you want to overwrite the default behaviour, you can easily do that using `'escaped' => false` on that column.
- If you're using the `markdown` or `custom_html` columns, please note that they still DO NOT escape the output by default (they really wouldn't make sense escaped); make sure you've properly sanitized your input or output; you can use an [HTML Purifier package](https://github.com/mewebstudio/Purifier) for that (do it [manually](https://github.com/Laravel-Backpack/demo/commit/7342cffb418bb568b9e4ee279859685ddc0456c1) or by casting the attribute to `CleanHtmlOutput::class`);


<a name="cache"></a>
### Cache

<a name="step-18" href="#step-18" class="badge badge-warning text-white" style="text-decoration: none;">Step 18.</a> Clear your app's cache:
```
php artisan config:clear
php artisan cache:clear
php artisan view:clear
```

---

**You're done! Good job.** Thank you for taking the time to upgrade. Now you can:
- thoroughly test your application and your admin panel;
- start using the [new features in Backpack 4.2](/docs/4.2/release-notes);
