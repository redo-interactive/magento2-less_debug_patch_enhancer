# Magento 2 Wikimedia Less PHP Enhanced Debugging Patch

Enhanced error reporting and debugging capabilities for the Wikimedia Less PHP library used in Magento.

## Compatible with

- Magento 2.4.8
- Wikimedia Less PHP v5.4
- PHP 8.3, 8.4

## Installation

To install patches with composer you need to have "cweagans/composer-patches" installed first. If you don't have it installed you can do it with:

```bash
composer require cweagans/composer-patches --dev
```

Then, add the patches to your composer.json:

```json
{
    "extra": {
        "patches": {
            "wikimedia/less.php": {
                "Enhanced Less compilation error reporting": "path_to_patch/wikimedia-less-php-consolidated-debugging-final.patch"
            }
        }
    }
}
```

Then run the commands:

```bash
composer install
php bin/magento cache:clean
```

## What is this patch?

When Less compilation fails in Magento 2, standard error messages provide minimal debugging information, making it difficult to identify and fix styling issues quickly. This is particularly problematic in development environments where CSS compilation errors can significantly slow down the development process.

Standard Less compilation errors in Magento provide minimal debugging information:
```
"math functions take numbers as parameters in _module.less"
```

This patch provides detailed, actionable error messages:
```
math function 'ceil' expects numeric parameters, got Less_Tree_Dimension in /app/design/frontend/MyTheme/custom/web/css/source/_module.less 
```

The patch enhances the Wikimedia Less PHP library used by Magento 2 to provide:

- **Full file paths** - Shows complete path instead of just filename
- **Specific function names** - Identifies which Less function failed  
- **Parameter type details** - Shows actual parameter types and values
- **Debug logging** - Comprehensive error context for troubleshooting
- **Index tracking** - Enhanced position tracking for accurate error reporting

## Verification

After installation, Less compilation errors will include enhanced debugging information. You can verify the patch is working by:

1. Checking vendor files contain the modifications:
```bash
grep -n "Debug: Log index and input info" vendor/wikimedia/less.php/lib/Less/Exception/Parser.php
```

2. Looking for enhanced error messages in logs during Less compilation
```bash
bin/magento setup:static-content:deploy
```

## Technical Details

The patch modifies three files in the Wikimedia Less PHP library:

1. **lib/Less/Exception/Parser.php** - Removes `basename()` to show full file paths and adds debug logging
2. **lib/Less/Functions.php** - Enhances math function error messages with parameter details  
3. **lib/Less/Tree/Call.php** - Updates Less_Functions instantiation to pass index parameter

## Troubleshooting

### Patch Fails to Apply
- Ensure you're using a compatible version of wikimedia/less.php (v5.4)
- Check that the vendor directory is clean before applying
- Verify the patch file path is correct in composer.json

### No Enhanced Error Messages  
- Clear Magento cache: `php bin/magento cache:clean`
- Check that the patch was applied by looking for modifications in vendor files
- Ensure error logging is enabled in your environment

## Acknowledgments

This patch addresses a common pain point in Magento 2 development where Less compilation errors lack sufficient context for quick debugging and resolution.

## Recommended Reading

- [Wikimedia Less PHP Documentation](https://github.com/wikimedia/less.php)
- [Composer Patches Documentation](https://github.com/cweagans/composer-patches) 
