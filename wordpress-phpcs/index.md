# Using PHPCS to manage coding standards in Wordpress

`composer init`

`composer require --dev dealerdirect/phpcodesniffer-composer-installer`

`composer require --dev phpcompatibility/php-compatibility`

`composer require --dev wp-coding-standards/wpc`

`code --install-extension ikappas.phpcs`

```json
{
    "require": {},
    "require-dev": {
        "dealerdirect/phpcodesniffer-composer-installer": "^0.5.0",
        "phpcompatibility/php-compatibility": "*",
        "wp-coding-standards/wpcs": "^2.1"
    },
    "scripts": {
		"install-codestandards": [
          "Dealerdirect\\Composer\\Plugin\\Installers\\PHPCodeSniffer\\Plugin::run"
        ],
        "phpcs": [
          "phpcs --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core app/"
        ],
        "phpcbf": [
          "phpcbf --standard=WordPress,WordPress-Extra,WordPress-Docs,WordPress-Core app/"
        ],
        "post-install-cmd": [
          "@install-codestandards"
        ]
	}
}
```
