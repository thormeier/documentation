# Composer Virtual Packages

Virtual packages are a way to specify the dependency on an implementation of an interface-only repository without forcing a specific implementation. For Httplug, the virtual package is called `php-http/client-implementation`. 

There is no project registered with that name. However, all client implementations including client adapters for Httplug use the `provide` section to tell composer that they do provide the client-implementation.


# Using a Reusable Library

Reusable libraries do not depend on a concrete implementation but only on the virtual package `php-http/client-implementation`. This is to avoid hard coupling and allows the user of the library to choose the implementation. You can think of this as an "interface" or "contract" for packages.

When *using a reusable library* in an application, one must `require` a concrete implementation. Make sure the `require` section includes a [package that provides php-http/client-implementation](https://packagist.org/providers/php-http/client-implementation). Failing to do that will lead to composer reporting this error:

``` bash
$ composer update
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - The requested package php-http/client-implementation could not be found in any version, there may be a typo in the package name.
```

Doing something like the following will make this problem go away:

``` bash
$ composer require php-http/guzzle6-adapter
```


# Building a Reusable Library

When *writing a reusable library*, the `require` section should only mention `php-http/client-implementation`, while the `require-dev` section needs to specify a concrete implementation in most cases, in order for the package to be installable on its own for development and testing.
