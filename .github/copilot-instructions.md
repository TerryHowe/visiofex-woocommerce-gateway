# VisioFex WooCommerce Gateway Plugin

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

The VisioFex WooCommerce Gateway is a PHP-based WordPress plugin that integrates WooCommerce with the VisioFex/KonaCash payment processing system. This plugin provides hosted checkout sessions, refund functionality, and WooCommerce Blocks support.

## Working Effectively

### Repository Structure Validation
Run these commands to validate the plugin structure and syntax:
```bash
# Navigate to plugin directory
cd /absolute/path/to/visiofex-woocommerce-gateway

# Validate PHP syntax for all files - takes ~120ms. NEVER CANCEL.
php -l visiofex-woocommerce-gateway.php
php -l includes/class-visiofex-gateway.php
php -l includes/class-wc-gateway-visiofex-blocks.php

# Check file structure integrity
ls -la assets/js/visiofex-redirect-fallback.js
ls -la assets/css/visiofex-payment-icons.css
ls -la assets/blocks/index.js
ls -la assets/visiofex-logo.png
```

### NO BUILD PROCESS REQUIRED
This plugin has NO build process. Do not attempt to:
- Run `npm install` or `npm run build` (no package.json exists)
- Run `composer install` (no composer.json exists)
- Use webpack, gulp, or any build tools
- Compile or transpile any files

All PHP, JavaScript, and CSS files are ready-to-use and directly loaded by WordPress.

### Testing and Validation
Since this is a WordPress plugin, testing requires a WordPress environment with WooCommerce:
```bash
# Basic validation (works without WordPress) - takes ~120ms total
php -l visiofex-woocommerce-gateway.php
find . -name "*.php" -exec php -l {} \;

# File integrity check
du -h . # Should show ~468KB total size
find . -type f | wc -l # Should show ~25+ files
```

**CRITICAL**: You cannot fully test this plugin without:
1. WordPress 6.0+ installation
2. WooCommerce 7.0+ plugin active
3. PHP 7.4+ environment
4. VisioFex API credentials for payment testing

### WordPress Plugin Installation
To install and test this plugin in a WordPress environment:
```bash
# Create plugin ZIP (if needed)
zip -r visiofex-woocommerce-gateway.zip . -x ".*" "*.git*" "*.DS_Store"

# Upload via WordPress admin:
# 1. Go to WordPress Admin -> Plugins -> Add New -> Upload Plugin
# 2. Upload the ZIP file
# 3. Activate the plugin
# 4. Configure via WooCommerce -> Settings -> Payments -> VisioFex Pay
```

## Validation

### Essential Validation Steps
ALWAYS run these validation steps before making changes:
```bash
# Syntax validation (required) - ~120ms. NEVER CANCEL.
php -l visiofex-woocommerce-gateway.php
php -l includes/*.php

# File permissions check
find . -name "*.php" -not -perm 644 # Should return nothing
find . -name "*.js" -not -perm 644  # Should return nothing
find . -name "*.css" -not -perm 644 # Should return nothing
```

### Manual Testing Requirements
When making changes to this plugin, you MUST validate using these scenarios in a WordPress environment:

1. **Plugin Activation Test**:
   - Activate plugin in WordPress admin
   - Verify no PHP errors in WordPress error log
   - Check that "VisioFex Pay" appears in WooCommerce payment methods

2. **Configuration Test**:
   - Navigate to WooCommerce -> Settings -> Payments -> VisioFex Pay
   - Enter test API credentials
   - Enable test mode
   - Save settings and verify no errors

3. **Checkout Flow Test** (requires WooCommerce environment):
   - Add product to cart
   - Proceed to checkout
   - Select VisioFex payment method
   - Verify payment form displays correctly with logo and card icons
   - Test redirect functionality (in test mode)

### Code Quality Requirements
Before completing any changes:
```bash
# PHP syntax validation - ~120ms. NEVER CANCEL.
find . -name "*.php" -exec php -l {} \;

# WordPress security check
grep -r "defined( 'ABSPATH' )" *.php includes/*.php
# Should find security checks in all PHP files

# Plugin header validation
head -30 visiofex-woocommerce-gateway.php | grep -E "(Plugin Name|Version|Requires)"
```

## Common Tasks

### File Structure
```
visiofex-woocommerce-gateway/
├── visiofex-woocommerce-gateway.php    # Main plugin file (62KB)
├── readme.txt                          # WordPress plugin readme
├── includes/                           # PHP class files
│   ├── class-visiofex-gateway.php      # Legacy gateway class
│   └── class-wc-gateway-visiofex-blocks.php # Blocks integration
├── assets/                             # Frontend assets
│   ├── js/                            # JavaScript files
│   ├── css/                           # Stylesheets
│   ├── images/                        # Card brand SVG icons
│   ├── blocks/                        # WooCommerce Blocks JS
│   └── visiofex-logo.png              # Plugin logo
└── languages/                         # Internationalization
    └── visiofex-woocommerce-gateway.pot
```

### Key Plugin Information
- **Plugin Name**: VisioFex for WooCommerce
- **Version**: 1.4.6  
- **WordPress**: Requires 6.0+, tested up to 6.6
- **PHP**: Requires 7.4+
- **WooCommerce**: Requires 7.0+, tested up to 9.2
- **License**: GPLv3

### Main Components
1. **Payment Gateway Class** (`WC_Gateway_VisioFex`): Core payment processing
2. **Blocks Integration** (`WC_Gateway_VisioFex_Blocks`): WooCommerce Blocks support  
3. **Admin Interface**: Settings, order management, refund processing
4. **Frontend Assets**: Checkout UI, redirect fallback system
5. **API Integration**: VisioFex/KonaCash payment API communication

### Configuration Requirements
For proper testing, the plugin requires:
- **Secret Key**: VisioFex API key
- **Vendor ID**: VisioFex application ID
- **Store Domain**: Your website URL (for return URLs)
- **Test Mode**: Enable for development/testing

### Debugging and Troubleshooting
Enable debug logging via plugin settings to access detailed logs at:
`WooCommerce -> Status -> Logs -> VisioFex`

Common validation commands:
```bash
# Check WordPress error logs (if WordPress installed)
tail -f /path/to/wordpress/wp-content/debug.log

# Validate plugin structure
find . -name "*.php" | wc -l  # Should be 3 files
find . -name "*.js" | wc -l   # Should be 3 files  
find . -name "*.css" | wc -l  # Should be 2 files
```

### Security Considerations
- All PHP files include WordPress security checks
- User input is sanitized using WordPress functions
- API communications use proper authentication headers
- Sensitive data is masked in logs

## Development Workflow

### Making Changes
1. **ALWAYS** validate PHP syntax before committing changes
2. Test in a WordPress/WooCommerce environment when possible
3. Check that assets load correctly (JS, CSS, images)
4. Verify no JavaScript console errors
5. Test the complete checkout flow if changing payment logic

### Critical Notes
- **NO BUILD TOOLS**: This plugin uses vanilla PHP, JS, and CSS
- **WordPress Dependency**: Cannot run standalone, requires WordPress/WooCommerce
- **API Testing**: Requires valid VisioFex credentials for full functionality testing
- **NEVER CANCEL**: PHP syntax validation takes ~120ms total - always wait for completion

### Plugin Size and Performance
- Total plugin size: ~468KB
- Main plugin file: 62KB
- Assets directory: ~405KB (including images and logos)
- PHP syntax validation: ~120ms for all files
- File operations: <1ms each

Remember: This plugin integrates with WordPress and WooCommerce core functionality. Always test changes in a proper WordPress environment to ensure compatibility and functionality.