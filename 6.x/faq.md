# Frequently Asked Questions

---


<a name="licensing"></a>
## Licensing

<a name="no-license-needed-on-localhost"></a>
### Do I need a license to test Backpack?

You don't need a license code AT ALL. Go head and install Backpack CRUD on your machine - it's free and open-source, released under the MIT License.

You only need to pay if you want the extra features provided by our premium add-ons (eg. [Backpack PRO](https://backpackforlaravel.com/pricing) and [Backpack DevTools](https://backpackforlaravel.com/products/devtools). That's it.


<a name="licese-for-staging-domain"></a>
### Do I need a license to put a PRO project on a testing domain?

No:
- when you purchase [Backpack PRO for Unlimited Projects](https://backpackforlaravel.com/products/pro-for-unlimited-projects), you can use it on any number of domains, subdomains and IPs (that's why it's called unlimited);
- when you purchase [Backpack PRO for One Project](https://backpackforlaravel.com/products/pro-for-one-project), you get the right to use it on one MAIN domain, but also as many staging/test/beta domains or subdomains as you need; if someone from our team contacts you, you can explain, it's perfectly reasonable to have test instances - we know how it goes;


<a name="backpack-license-for-open-source-projects"></a>
### Can I use Backpack to create an open-source project?

Yes you can! Use [Backpack CRUD v6](https://github.com/laravel-backpack/crud), which is free & open-source, released under the MIT License.


<a name="backpack-pro-for-open-source-projects"></a>
### Can I use Backpack PRO in an open-source project?

In short - no, you cannot. Please use [Backpack CRUD v6](https://github.com/laravel-backpack/crud) instead, which is free & open-source, released under the MIT License.

Backpack PRO is a closed-source add-on, which requires payment in order to receive an access token. If you did include `backpack/pro` as a dependency in your open-source software... your software would no longer be open-source. Everybody who installed your project/package would need to pay for Backpack PRO, to get access to it.


<a name="backpack-pro-for-non-commercial-projects"></a>
### Can I get Backpack PRO for free, to use in a non-commercial project?

No - we're no longer giving away free licenses. But we _have_ released Backpack CRUD v5 and v6 under MIT License, which means it's free & open-source. It has fewer features, but you can do absolutely anything you want with it.

<a name="miscellaneous"></a>
## Miscellaneous


<a name="how-do-i-update-backpack"></a>
### How do I update Backpack to the latest non-breaking version?

First of all, **run `composer update` on your project**. That will pull in the latest supported version of all your Backpack packages.

Then you should **re-publish the JS and CSS assets**. There are two ways to do that:

(A) Run `php artisan vendor:publish --provider="Backpack\CRUD\BackpackServiceProvider" --tag=public --force`. Please note this will overwrite anything that's already there. This B solution has a downside: _unused files are not removed_. A few files Backpack no longer uses will still be in your public/packages folder, even though they're no longer used.

(B) If you have NOT touched you `public/packages` folder, or placed anything custom inside it:
- delete the public/packages directory and all its contents;
- run `php artisan vendor:publish --provider="Backpack\CRUD\BackpackServiceProvider" --tag=public`
- if you use elFinder, also delete `resources/views/vendor/elfinder` and run `php artisan backpack:filemanager:install`


<a name="how-do-i-uninstall-backpack"></a>
### How do I uninstall Backpack from my project?

You can remove Backpack from your project pretty easily, if you decide to stop using it. You just have to do the opposite of the installation process:

```bash
# delete the files Backpack has placed inside your application
rm -rf app/Http/Middleware/CheckIfAdmin.php
rm -rf config/backpack
rm -rf config/gravatar.php
rm -rf public/packages
rm -rf resources/views/errors
rm -rf resources/views/vendor/backpack
rm -rf routes/backpack

# delete any CrudControllers you've created, so MAYBE:
rm -rf app/Http/Controllers/Admin

# delete any Requests you've created for your CrudControllers.
# MAKE SURE YOU DON'T NEED ANYTHING IN THIS DIRECTORY ANYMORE.
# You might have OTHER requests that are not Backpack-related.
rm -rf app/Http/Requests

# (MUST) remove other Backpack packages that you are using, like PRO, Editable Columns, DevTools etc:
composer remove --dev backpack/devtools
composer remove backpack/pro
composer remove backpack/editable-columns

etc...

# After everything related to Backpack is deleted, just need to delete the crud!
composer remove backpack/crud

```

That's it! If you've decided NOT to use Backpack, we'd be super-grateful if you could <a href="mailto:hello@backpackforlaravel.com?subject=Why%20I%20decided%20against%20using%20Backpack&body=Hey%20there%20Backpack%20team%2C%0D%0A%0D%0AHopefully%20my%20experience%20will%20help%20you%20take%20Backpack%20in%20a%20direction%20where%20it'll%20be%20a%20better%20fit%20for%20me%20in%20the%20future.%0D%0A%0D%0AI've%20decided%20to%20NOT%20use%20Backpack%20in%20my%20project%20because...">send us an email</a> telling us WHY you didn't like Backpack, or it didn't fit your project. It might help us take Backpack in a different direction, one where you might want to use it. Thank you 🙏
