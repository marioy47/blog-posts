# How to install phpcs Globally and configure Visual Studio Code to use it

## Install Globally

Latest instructions here: https://github.com/squizlabs/PHP_CodeSniffer

`composer global require --dev "squizlabs/php_codesniffer=*"`

`which phpcs`

`where.exe phpcs`


`phpcs --config-set default_standard PSR12`

## Install WordPress Coding Standards

```bash
composer require --dev \
    "dealerdirect/phpcodesniffer-composer-installer=*" \
    "phpcompatibility/phpcompatibility-wp:*" \
    "wp-coding-standards/wpcs=*" \
    "sirbrillig/phpcs-variable-analysis:*"
```

Go to [Automatics Jetpack](https://github.com/Automattic/jetpack/) project and download


- `.phpcs.xml.dist` https://github.com/Automattic/jetpack/blob/master/.phpcs.xml.dist
- `.editorconfig`: https://github.com/Automattic/jetpack/blob/master/.editorconfig
- `.prettierrc`: https://github.com/Automattic/jetpack/blob/master/.prettierrc
