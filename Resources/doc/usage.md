Usage
====

In order to have a list of available path to manage through web interface, you must load them in CopiaincollaMetaTagsBundle. You can do it in one of theese ways:

- _load all the routes of a bundle_ by including the bundle name under ci_metatags_bundle in `config.yml`
- use "ci_metatags_expose" @Route annotation option on specific route
- _load dynamic routes_ (eg. referred to some entities) setting the required parameters in `config.yml`
- generate paths through a `custom service`

---

## Include bundle name under `ci_metatags_bundle`

This method loads all the paths generated from routes contained in a list of bundles. It is useful either to load non parametric paths, or in combination with `fixed_params` configuration parameter (see [configuration reference](Resources/doc/configuration.md#copiaincolla_meta_tags--urls_loader--exposed_routes)).

In `config.yml`, by setting:

```
copiaincolla_meta_tags:
    urls_loader:
        exposed_routes:
            bundles:
                - FooBarBundle
                - FooOtherBundle
```

all paths generated from routes in controllers belonging to `FooBarBundle` and `FooOtherBundle` are loaded. If a route throws an exception (eg. because it needs a parameter), the relative path is not loaded.

## use "ci_metatags_expose" @Route annotation option on specific route

You can add a single route by specifying an option in the Route annotation, like this:

```
@Route("/contact", name="contact", options={"ci_metatags_expose"=true})
```

Through this option, you can also choose __not__ to generate urls from a specific route:

```
@Route("/contact", name="contact", options={"ci_metatags_expose"=false})
```

## load dynamic routes (eg. referred to some entities) setting the required parameters in `config.yml`

Frequently you will need to generate paths which have some association with entities or datas in database.


To generate paths by fetching data from database, read the section [Configuration#dynamic_routes](Resources/doc/configuration.md#copiaincolla_meta_tags--urls_loader--parameters--dynamic_routes).

A typical example is when you have a route like

```
@Route("/product/{code}", name="product_show")
```

and you want to automatically generate all the paths for products stored in database.

## Display help message to user

You can specify a @Route option to display a message in the edit page, just use the `ci_metatags_help` options:

```
@Route("/product/{id}/{slug}", name="product_show", options={"ci_metatags_help"="This urls represents a product. Use {{ entity.title }} to print the title of a product"})
```


## Usage in the templates

Currently only twig is supported.

In the template containing an `<head>` tag, simply add:

    ```
<body>
<head>
    {% render controller('CopiaincollaMetaTagsBundle:MetaTags:render') %}
    [...]
</head>

[...]
</body>
```

To overwrite the default meta tags and the custom meta tags inserted by web interface, you can specify the `inlineMetatags` variable:

```
{% render controller('CopiaincollaMetaTagsBundle:MetaTags:render', { 'inlineMetatags': {'title': 'New foo title'} }) %}
```


For a better explanation of usage in templates and advanced use, read [Template usage](Resources/doc/template_usage.md).