# WooCommerce Tooltip for Shipping Methods

This code adds a tooltip next to each shipping option in the WooCommerce checkout. It displays additional information when hovering over the question mark icon, enhancing user experience with best-practice tooltip design.

## Features

- **Concise, Clear Information**: The tooltip provides essential details only.
- **Placement Next to Element**: Tooltip appears beside the icon for easy readability.
- **Easy Closure**: Disappears on mouse leave.
- **Mobile Adaptability**: Resizes to fit mobile screens.

## Installation

1. Add this code to your `functions.php` file or create a custom plugin.
2. Adjust tooltip content as needed.

## Screenshot

![Tooltip Example](path/to/your/screenshot.png)

> **Note**: Replace `path/to/your/screenshot.png` with the actual path to your screenshot file.

## Code

```php
add_action( 'woocommerce_after_shipping_rate', 'ecommercehints_output_shipping_method_tooltips', 10 );
function ecommercehints_output_shipping_method_tooltips( $method ) {
    $meta_data = $method->get_meta_data();
    if ( array_key_exists( 'description', $meta_data ) ) {
        $description = apply_filters( 'ecommercehints_description_output', html_entity_decode( $meta_data['description'] ), $method );
        if ($description) {
            echo '<div class="tooltip-container" style="display: inline-block; margin-left: 5px; position: relative;">
                    <span class="tooltip-trigger" style="font-size: 12px; color: #333; cursor: pointer; width: 16px; height: 16px; display: inline-flex; align-items: center; justify-content: center; border-radius: 50%; border: 1px solid #333;">?</span>
                    <span class="tooltip-text" style="display: none; background-color: #f9f9f9; color: #333; padding: 8px; border-radius: 4px; position: absolute; top: 100%; left: 50%; transform: translateX(-50%); white-space: normal; max-width: 250px; font-size: 12px; line-height: 1.4; box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2); z-index: 10000;">
                        ' . wp_kses( $description, wp_kses_allowed_html( 'post' ) ) . '
                    </span>
                  </div>';
        }
    }
}

add_action('wp_head', 'ecommercehints_tooltip_css');
function ecommercehints_tooltip_css() { ?>
    <style>
        .tooltip-container:hover .tooltip-text {
            display: block !important;
        }
        .tooltip-container .tooltip-text {
            overflow-wrap: break-word;
            max-width: 90vw;
        }
        @media (max-width: 600px) {
            .tooltip-container .tooltip-text {
                left: 0;
                transform: none;
                max-width: 85vw;
            }
        }
    </style>
<?php
}