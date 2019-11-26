---
title: Using PHP_CodeSniffer to manage coding standards for WordPress Development
tags: phpcs, phpcbf, wordpress, vscode, composer, php
---

# Using PHP_CodeSniffer to manage coding standards for WordPress Development

## Start the project

```bash
composer init
```

I didn't defined dependencies just yet.

```composer.json
{
    "name": "marioy47/wordpress-phpcs",
    "description": "Test for phpcs in wordpress",
    "type": "project",
    "authors": [
        {
            "name": "Mario Yepes",
            "email": "marioy47@gmail.com"
        }
    ],
    "require": {}
}
```

## Install PHPCS Composer Installer

```shell
composer require --dev 'dealerdirect/phpcodesniffer-composer-installer:*'
```

Add the following scripts to `composer.json`

```composer.json
{
  // ...
  "scripts": {
    "install-codestandards": [
      "Dealerdirect\\Composer\\Plugin\\Installers\\PHPCodeSniffer\\Plugin::run"
    ],
    "post-install-cmd": [
      "@install-codestandards"
    ]
  }
}
```

## Install PHPCS Validators

We'll be installing

- PHP Version Compatibility Checks
- Wordpress Compatibility Test
- Empty and unused variables analysis
- Wordpress Coding Standards

```shell
composer require --dev 'phpcompatibility/php-compatibility:*' \
  'phpcompatibility/phpcompatibility-wp:*'  \
  'sirbrillig/phpcs-variable-analysis:*' \
  'wp-coding-standards/wpcs:*'
```

Add the following in the **scripts** section in `composer.json`

```composer.json
{
  // ..
  "scripts": {
    // Add this three commands.
    "phpcs": "phpcs",
    "phpcsi": "phpcs -i",
    "phpcbf": "phpcbf"
  }
}
```

To validate that you have installed all the required standards you should execute `phpcs -i` but since we have _phpcs_ installed only locally we created the `composer phpcs-i` command. That will list the **locally** installed standards.

```shell
$ composer phpcsi
> phpcs -i
The installed coding standards are PEAR, Zend, PSR2, MySource, Squiz, PSR1, PSR12, PHPCompatibility, PHPCompatibilityParagonieRandomCompat, PHPCompatibilityParagonieSodiumCompat, PHPCompatibilityWP, VariableAnalysis, WordPress, WordPress-Extra, WordPress-Docs and WordPress-Core
```

## Our first test

Create a REALLY BAD php file:

```phpcs-test.php
<?php

function my_FunctionOne()
{ $a = null;
$b;
    // This is a test
return ""}

?>
```

and in the terminal run `composer phpcs` to verify that its workgin. You should get something like:

```shell
$ composer phpcs phpcs-test.php
> phpcs 'phpcs-test.php'

FILE: /Users/Mario/Projects/blog-posts/wordpress-phpcs/phpcs-test.php
----------------------------------------------------------------------
FOUND 8 ERRORS AFFECTING 5 LINES
----------------------------------------------------------------------
 2 | ERROR | [ ] Missing file doc comment
 3 | ERROR | [ ] Function name "my_FunctionOne" is prefixed with a
   |       |     package name but does not begin with a capital
   |       |     letter
 3 | ERROR | [ ] Function name "my_FunctionOne" is invalid; consider
   |       |     "My_functionOne" instead
 3 | ERROR | [ ] Missing doc comment for function my_FunctionOne()
 4 | ERROR | [x] Opening brace must be the last content on the line
 5 | ERROR | [x] Line indented incorrectly; expected at least 4
   |       |     spaces, found 0
 7 | ERROR | [x] Line indented incorrectly; expected at least 4
   |       |     spaces, found 0
 7 | ERROR | [x] Closing brace must be on a line by itself
----------------------------------------------------------------------
PHPCBF CAN FIX THE 4 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------

Time: 45ms; Memory: 4MB

Script phpcs handling the phpcs event returned with error code 2
```

If you are familiar with Wordpress Standards, you can see that the WP Coding Standards are not being applied. To that we have to create 2 new commands.

## Using Wordpress Standards

Add the following 2 commands to the **scripts** section in the `composer.json` file.

```composer.json
{
  // ...
  "scripts": {
    // ...
    "phpcs-wp": [
      "phpcs --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core"
    ],
    "phpcbf-wp": [
      "phpcbf --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core"
    ]
  }
}

```

And execute `composer phpcs-wp phpcs-test.php` to make the check but using WordPress standards

```bash
$ composer phpcs-wp phpcs-test.php
> phpcs --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core 'phpcs-test.php'

FILE: /Users/Mario/Projects/blog-posts/wordpress-phpcs/phpcs-test.php
----------------------------------------------------------------------
FOUND 12 ERRORS AFFECTING 6 LINES
----------------------------------------------------------------------
 1 | ERROR | [ ] Missing file doc comment
 3 | ERROR | [x] Expected exactly one space between closing
   |       |     parenthesis and opening control structure; "
   |       |     " found.
 3 | ERROR | [ ] Function name "my_FunctionOne" is not in snake case
   |       |     format, try "my_function_one"
 3 | ERROR | [ ] Missing doc comment for function my_FunctionOne()
 4 | ERROR | [x] Opening brace should be on the same line as the
   |       |     declaration
 4 | ERROR | [x] Opening brace must be the last content on the line
 5 | ERROR | [x] Line indented incorrectly; expected at least 1 tabs,
   |       |     found 0
 6 | ERROR | [x] Tabs must be used to indent lines; spaces are not
   |       |     allowed
 6 | ERROR | [ ] Inline comments must end in full-stops, exclamation
   |       |     marks, or question marks
 7 | ERROR | [x] Line indented incorrectly; expected at least 1 tabs,
   |       |     found 0
 7 | ERROR | [ ] PHP syntax error: syntax error, unexpected '}',
   |       |     expecting ';'
 7 | ERROR | [x] String "" does not require double quotes; use single
   |       |     quotes instead
----------------------------------------------------------------------
PHPCBF CAN FIX THE 7 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------

Time: 207ms; Memory: 8MB

Script phpcs --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core handling the phpcs-wp event returned with error code 2
```

NOW we are applying WordPress standards!.

But there are some thing that we're still not validating, like _language namespace_. And adding those options here is overkill.

That's why we'll be using a configuration file.

## PHPCS configuration using xml file

Next, there is a minimal configuration that does the following:

- Verifies that we're using Wordpress <= 5.1 and PHP >= 7.0
- That the text domain is `text-domain`
- All files to verify are in `/app` and `plugin-start-file.php``
- Ignores everything in `/Docker`, `/vendor`, `/test`, `/node_modules`

```phpcs.xml
<?xml version="1.0"?>
<ruleset name="Wordpress Plugin">
  <config name="minimum_supported_wp_version" value="5.1" />
  <config name="testVersion" value="7.0-"/>

  <rule ref="PHPCompatibilityWP"/>
  <rule ref="WordPress-Core">
    <exclude name="WordPress.Files.FileName.NotHyphenatedLowercase" />
    <exclude name="WordPress.Files.FileName.InvalidClassFileName" />
  </rule>
  <rule ref="WordPress-Docs" />
  <rule ref="WordPress-Extra" />
  <rule ref="VariableAnalysis" />
  <rule ref="WordPress.WP.I18n">
    <properties>
      <!-- Change here your text-domain -->
      <property name="text_domain" type="array" value="text-domain" />
    </properties>
	</rule>
  <rule ref="Squiz.PHP.CommentedOutCode.Found">
    <severity>0</severity>
  </rule>

  <!-- Configure here the paths you want to check -->
  <file>plugin-start-file.php</file>
  <file>app/</file>

  <!-- Show sniff codes in all reports -->
  <arg value="sp"/> <!-- Show sniff and progress -->
  <arg name="parallel" value="8"/> <!-- Enables parallel processing when available for faster results. -->
  <arg name="colors"/>
  <arg name="extensions" value="php"/>

  <!-- Configure here the paths you want to IGNORE -->
  <exclude-pattern>/docker/*</exclude-pattern>
  <exclude-pattern>/node_modules/*</exclude-pattern>
  <exclude-pattern>/tests/*</exclude-pattern>
  <exclude-pattern>/vendor/*</exclude-pattern>
</ruleset>
```

Now you can execute `composer phpcs` without any arguments and it will validate against Wordpress and any additional rule you've configured

## Visual Studio Code

There are a couple of extensions that work with `phpcs` but at the time of this writing the most updated one was `wongjn.php-sniffer` which can be installed with the following command:

```install.sh
code --install-extension wongjn.php-sniffer
```

And since you are using a locally installed `phpcs` you should make the following configuration in _Visual Studio Code_ by editing the file `.vscode/settings.json` and adding the following:

```.vscode/settings.json
{
  // ...
  "[php]": {
    "editor.defaultFormatter": "wongjn.php-sniffer"
  },
  "phpSniffer.executablesFolder": "vendor/bin/"
}
```

## Starter Project

Since this are too many files to take care of, and many commands just to start a project, I've created a [Wordpress Starter Project](https./)
