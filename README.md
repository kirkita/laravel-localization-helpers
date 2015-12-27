Laravel Localization Helpers
============================

[![Latest Stable Version](https://poser.pugx.org/potsky/laravel-localization-helpers/v/stable.svg)](https://packagist.org/packages/potsky/laravel-localization-helpers)
[![Latest Unstable Version](https://poser.pugx.org/potsky/laravel-localization-helpers/v/unstable.svg)](https://packagist.org/packages/potsky/laravel-localization-helpers)
[![Total Downloads](https://poser.pugx.org/potsky/laravel-localization-helpers/downloads.svg)](https://packagist.org/packages/potsky/laravel-localization-helpers)
[![Build Status](https://travis-ci.org/potsky/laravel-localization-helpers.svg)](https://travis-ci.org/potsky/laravel-localization-helpers)
[![Coverage Status](https://coveralls.io/repos/potsky/laravel-localization-helpers/badge.svg?branch=master&service=github)](https://coveralls.io/github/potsky/laravel-localization-helpers?branch=master)


LLH is a set of tools to help you manage translations in your Laravel project.

## 1. Installation

- Add the following line in the `require-dev` array of the `composer.json` file :
    ```
    "potsky/laravel-localization-helpers" : "~1"
    ```

- Update your installation : `composer update`
- Add the following line in the `providers` array of the `app/config/app.php` configuration file :
    ```
    'Potsky\LaravelLocalizationHelpers\LaravelLocalizationHelpersServiceProvider'
    ```

- Now execute `php artisan list` and you should view the new *localization* commands:
    ```
    ...
    localization
    localization:clear          Remove all backups
    localization:find           Display all files where the argument is used as a lemma
    localization:missing        Parse all translations in app directory and build all lang files
    ...
    ```

## 2. Configuration

To configure your fresh installed package, please create a configuration file by executing :

- Laravel 4: `php artisan config:publish potsky/laravel-localization-helpers`
- Laravel 5: `php artisan vendor:publish`

Then you can modify the configuration in file `app/config/packages/potsky/laravel-localization-helpers/config.php` (Laravel 4) or `app/config/laravel-localization-helpers.php` (Laravel 5).

Add new folders to search for, add your own lang methods or functions, ...

### Backup files

You should not include backup lang files in GIT or other versioning systems.

In your `laravel` folder, add this in `.gitignore` file :

```
# Do not include backup lang files
app/lang/*/[a-zA-Z]*.[0-9_]*.php
```

## 3. Usage

### 3.1 Command `localization:missing`

This command parses all your code and generate according lang files in all `lang/XXX/` directories.

Use `php artisan help localization:missing` for more informations about options.

#### *Examples*

##### Generate all lang files

```
php artisan localization:missing
```

##### Generate all lang files without prompt

```
php artisan localization:missing -n
```

##### Generate all lang files without backuping old files

```
php artisan localization:missing -b
```

##### Generate all lang files without keeping obsolete lemmas

```
php artisan localization:missing -o
```

##### Generate all lang files without any comment for new found lemmas

```
php artisan localization:missing -c
```

##### Generate all lang files without header comment

```
php artisan localization:missing -d
```

##### Generate all lang files and set new lemma values

3 commands below produce the same output:
```
php artisan localization:missing
php artisan localization:missing -l
php artisan localization:missing -l "%LEMMA"
```

You can customize the default generated values for unknown lemmas.

The following command let new values empty:

```
php artisan localization:missing -l ""
```

The following command prefixes all lemmas values with "Please translate this : "

```
php artisan localization:missing -l "Please translate this : %LEMMA"
```

The following command prefixes all lemmas values with "Please translate this !"

```
php artisan localization:missing -l 'Please translate this !'
```

##### Silent option for shell integration

```
#!/bin/bash

php artisan localization:missing -s
if [ $? -eq 0 ]; then
echo "Nothing to do dude, GO for release"
else
echo "I will not release in production, lang files are not clean"
fi
```

##### Simulate all operations (do not write anything) with a dry run

```
php artisan localization:missing -r
```

##### Open all must-edit files at the end of the process

```
php artisan localization:missing -e
```

You can edit the editor path in your configuration file. By default, editor is *Sublime Text* on *Mac OS X* :

```
'editor_command_line' => '/Applications/Sublime\\ Text.app/Contents/SharedSupport/bin/subl'
```

### 3.2 Command `localization:find`

This command will search in all your code for the argument as a lemma.

Use `php artisan help localization:find` for more informations about options.

#### *Examples*

##### Find regular lemma

```
php artisan localization:find Search
```

##### Find regular lemma with verbose

```
php artisan localization:find -v Search
```

##### Find regular lemma with short path displayed

```
php artisan localization:find -s "Search me"
```

##### Find lemma with a regular expression

```
php artisan localization:find -s -r "@Search.*@"
php artisan localization:find -s -r "/.*me$/"
```

> PCRE functions are used

### 3.3 Command `localization:clear`

This command will remove all backup lang files.

Use `php artisan help localization:clear` for more informations about options.

#### *Examples*

##### Remove all backups

```
php artisan localization:clear
```

##### Remove backups older than 7 days

```
php artisan localization:clear -d 7
```

## 4. Support

Use the [github issue tool](https://github.com/potsky/laravel-localization-helpers/issues) to open an issue or ask for something.

## 5. Change Log

### v1.4

- new command to remove backups
- new options to specify output formatting
- new lemma are now marked with the TODO: prefix 
- simplified service provider, no need to load distinct providers according to Laravel version 

Internally :

- totally refactored
- unit tests
- test coverage

### v1.3.2

- fix incompatibility with Laravel 5.2 [#16](https://github.com/potsky/laravel-localization-helpers/issues/16)

### v1.3.1

- add resource folder for Laravel 5

### v1.3

- add full support for Laravel 5

### v1.2.2

- add support for @lang and @choice in Blade templates (by Jesper Ekstrand)

### v1.2.1

- add `lang_folder_path` parameter in configuration file to configure the custom location of your lang files
- check lang files in `app/lang` by default for Laravel 4.x
- check lang files in `app/resources/lang` by default for Laravel 5

### v1.2

- support for Laravel 5 (4.3)
- add `ignore_lang_files` parameter in configuration file to ignore lang files (useful for `validation` file for example)

