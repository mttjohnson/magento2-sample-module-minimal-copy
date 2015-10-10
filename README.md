# magento2-sample-module-minimal-copy
Magento 2 - Sample Module - Demonstrating the minimal requirements for using composer to copy files to app/code

## Installation Steps

    composer config repositories.mttjohnson-module-sample-minimal-copy git git@github.com:mttjohnson/magento2-sample-module-minimal-copy.git
    composer require mttjohnson/module-sample-minimal-copy:dev-master
    bin/magento module:enable Mttjohnson_SampleMinimalCopy --clear-static-content
    bin/magento setup:upgrade


## Breakdown

    composer config repositories.mttjohnson-module-sample-minimal-copy git git@github.com:mttjohnson/magento2-sample-module-minimal-copy.git
This will use composer to register a new repository [`composer config repositories`](https://getcomposer.org/doc/03-cli.md#modifying-repositories). Using the command line tool to register a new repository we have to specify a repository name `repositories.mttjohnson-module-sample-minimal-copy` so I chose to make it similar to the name of my composer package so it's easier to associate a repository with the package it's used for. The type of repository is [`git`](https://getcomposer.org/doc/05-repositories.md#git-alternatives) and then the repository location you have access to `git@github.com:mttjohnson/magento2-sample-module-minimal-copy.git`.

    composer require mttjohnson/module-sample-minimal-copy:dev-master
Here we use composer to add a new requirement to the project [`composer require`](https://getcomposer.org/doc/03-cli.md#require) and specify the name of the package `mttjohnson/module-sample-minimal-copy` and then specify what version we want `dev-master` and we use the speical [`dev-`](https://getcomposer.org/doc/02-libraries.md#branches) prefix to specify a branch name of `master`.
The composer require command here does a clone of your git repo and places that clone in `vendor/mttjohnson/module-sample-minimal-copy`. Because the composer type is [`magento2-module`](https://github.com/magento/magento-composer-installer#magento-module) in the composer.json file, it will run the composer customer installer that is implemented by [`magento-composer-installer`](https://github.com/magento/magento-composer-installer). The `extra/map` section of the composer.json file provides a map of files in your composer package to where they should be copied into the magento installation directory. In this case because it's a `magento2-module` the installer will copy files from `vendor/mttjohnson/module-sample-minimal-copy` to `app/code/Mttjohnson/SampleMinimalCopy`. When it copies the files the git repository files `.git` in the `vendor` directory do not get copied over to the `app/code` location and the files that Magento runs against will no longer be a part of your local cloned repository. This copy operation breaks the connection of your running code to your repository, and isn't suited very well for contributing back to the repository the module came from.

    bin/magento module:enable Mttjohnson_SampleMinimalCopy --clear-static-content
This will enable the module with Magento and confirm that Magento knows that the module exists and is able to use it.

    bin/magento setup:upgrade
Required after enabling a module to make sure any setup scripts are executed for the module and that the module version gets registered in the database.
