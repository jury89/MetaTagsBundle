MetaTagsBundle
==============

CopiaincollaMetaTagsBundle is a Symfony2 bundle built to help you manage html meta tags for SEO purposes.

# How it works

Given a path (eg. __"/en/contact"__), you can specify the html meta tags you want to be displayed in the related web page.

This is the list of meta tags currently supported by CopiaincollaMetaTagsBundle:

- __title__
- __description__
- __keywords__
- __author__
- __language__
- __robots__
- __googlebot__
- __og:title__
- __og:description__
- __og:image__


You, as developer, specify the paths you want to be managed. Then through web interface you can manually set meta tags values for a concrete path, or define some regex rules to be applied to paths. You can also set meta tags values in twig templates.

When a url is requested, it happens the following:

- path is extracted (eg. __"/en/contact"__) and used to calculate the right meta tags to display
- for each supported meta tag:
 - if the exact path exists in database, load stored value
 - else, regex rules matching the given path are applied (in order of importance); the first non-null value for that meta tag is loaded
 - else, value is loaded from twig template
 - else, meta tag is not displayed

### Changelogs

20/08/2013 - Version 1.1

Most important introduction is the possibility to define cascading regex rules for default meta tag values.

05/04/2013 - Version 1.0

In this date the version 1.0 is tagged, hurray! This means that this bundle is no more in beta release, but ready for production environments.

24/01/2013 - Version 0.1

THe work on this bundle begins, now in alpha state.

---

# How to install

The `master` branch and the `X.Y` tags are compatible with `Symfony >= 2.2.*`.

To choose the right version of this bundle to install, have a look at [Tagging and Branching system explanation.](tagging_branching.md)

Read more about tags [here](Resources/doc/tagging_branching.md).

### 1a) using composer
If you are using `composer`, add the following line to your `composer.json`:

```
{
    "require": {
        "copiaincolla/metatags-bundle": "X.Y"
    }
}
```

NOTE: substitute `"X.Y"` with the most recent tag or the concrete tag you want to use (view the list of available tags [here](https://github.com/copiaincolla/MetaTagsBundle/tags)).

Then run:

```
php composer.phar update
```
### 1b) using deps

If you are using `deps`, add the following line to your `deps`:

```
[CopiaincollaMetaTagsBundle]
    git=https://github.com/copiaincolla/MetaTagsBundle.git
    target=/bundles/Copiaincolla/MetaTagsBundle
    version=X.Y
```

And in your `app/autoload.php`:

```
'Copiaincolla'   => __DIR__.'/../vendor/bundles',
```

### 2) AppKernel.php

Load the bundle by adding this to `app/AppKernel.php`:

    new Copiaincolla\MetaTagsBundle\CopiaincollaMetaTagsBundle(),

### 3) Import routing

Add the following line to `app/routing.yml`:

```
# CopiaincollaMetaTagsBundle
ci_metatags_bundle:
    resource: "@CopiaincollaMetaTagsBundle/Resources/config/routing.yml"
```

#### 4) Add copiaincolla_meta_tags configuration

Add to `app/config.yml`:

```
copiaincolla_meta_tags: ~
```

#### 5) Update database

Update the database schema by running:

```
php app/console doctrine:schema:update --force
```

The bundle is now ready to work!

---

#  Usage

No path is managed by default by MetaTagsBundle: you need to __load__ the paths and associate (by web interface) the proper meta tags values.

There are some methods you can use to __load__ the paths in MetaTagsBundle:

- _load all the routes of a bundle_ by including the bundle name in `config.yml`
- _load dynamic routes_ (eg. referred to some entities) setting the required parameters in `config.yml`
- _for each route_, set the option `ci_metatags_expose` in the `Route annotation`
- generate paths through a `custom service`

To start using this bundle, follow the [Usage](Resources/doc/usage.md) documentation.

# Resources

[How to use this bundle](Resources/doc/usage.md)

[Configuration reference](Resources/doc/configuration.md)

Cookbook:

- [Use a custom service to load paths](Resources/doc/custom_urls_loader_service.md)
- [Console usage](Resources/doc/console.md)
- [Change the path of restricted area](Resources/doc/custom_admin_prefix.md)