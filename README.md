# JetFormBuilder Any Custom Icon Enhancer

This lightweight snippet enhances JetFormBuilder's submit buttons by allowing users to add custom icons from any icon library. The user specifies the icon classes in the block's "Additional CSS Class(es)" field, and the snippet automatically inserts the icon in the frontend.

## Features

- Supports any icon library, including [Flaticon Uicons](https://www.flaticon.com/uicons), [Font Awesome](https://fontawesome.com/), or any other.
- Dynamically injects icons into JetFormBuilder's submit buttons using custom CSS classes.
- Aligns icons with button text automatically.
- Removes unwanted `::before` symbols that may conflict with custom styles.
- Lightweight and easy to implement.

## How It Works

1. In Gutenberg, add a **Submit** block to your JetFormBuilder form.
2. In the block's settings (sidebar), find the "Additional CSS Class(es)" field.
3. Add a class in the format `icon-{library-specific-classes}`. Examples:
   - For Flaticon Uicons: `icon-fi fi-rr-envelope`
   - For Font Awesome: `icon-fa fa-paper-plane`
   - For other libraries, simply use the required class syntax (e.g., `icon-custom-icon`).
4. Ensure the icon library is enqueued on your site. If it's not already loaded, you can enqueue it manually (instructions below).
5. Save the form and view it on the frontend. The icon will appear before the button label, styled and aligned automatically.

## Installation

### Add the Snippet

1. Copy the following PHP code into your theme's `functions.php` file or use a plugin like [Code Snippets](https://wordpress.org/plugins/code-snippets/).

    ```php
    /**
     * Enqueue icon libraries and handle custom icons in JetFormBuilder submit buttons.
     */
    function enqueue_fi_icons_library() {
        // Example: Enqueue Flaticon Uicons (replace with your library if needed)
        wp_enqueue_style(
            'icon-library',
            'https://cdn-uicons.flaticon.com/2.6.0/uicons-regular-rounded/css/uicons-regular-rounded.css',
            array(),
            '2.6.0'
        );
    
        // Inline CSS: Remove unwanted ::before symbols and style icons
        $custom_css = '
        .jet-form-builder__action-button::before {
            content: none !important;
        }
        .icon-fi.fi-rr-envelope {
            display: inline-flex;
            align-items: center;
            vertical-align: middle;
        }
        i.fi.fi-rr-envelope {
            margin-right: 5px;
            align-items: center;
            display: flex;
        }';
    
        wp_add_inline_style('icon-library', $custom_css);
    }
    add_action('wp_enqueue_scripts', 'enqueue_fi_icons_library');
    
    add_filter('the_content', 'any_custom_icon_in_jetform_button', 20);
    function any_custom_icon_in_jetform_button($content) {
        $regex = '/(<button\b[^>]*icon-([a-z0-9- ]+)[^>]*>)(.*?)(<\/button>)/is';
    
        $content = preg_replace_callback($regex, function($matches) {
            $button_tag = $matches[1];
            $icon_name = esc_attr(trim($matches[2]));
            $button_content = $matches[3];
            $button_close = $matches[4];
    
            $icon_html = '<i class="' . $icon_name . '"></i>';
    
            return $button_tag . $icon_html . $button_content . $button_close;
        }, $content);
    
        return $content;
    }

## Enqueue Other Icon Libraries
If your site doesn't already include the desired icon library, you can enqueue it. Below is an example for Font Awesome:

    ```php    
    function enqueue_font_awesome() {
        wp_enqueue_style(
            'font-awesome',
            'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css',
            array(),
            '5.15.4'
        );
    }
    add_action('wp_enqueue_scripts', 'enqueue_font_awesome');

Replace the URL with the appropriate stylesheet URL for your icon library.

## Usage
Add the icon- prefix followed by the icon classes in the Submit block's "Additional CSS Class(es)" field.

## Example:
For Flaticon Uicons: icon-fi fi-rr-envelope
For Font Awesome: icon-fa fa-check
For any other library, provide the full class names as needed.
Save and view your form on the frontend to see the icon before the button text.

## Example
For a button with the text "Submit" and a Flaticon Uicons envelope icon:

Add icon-fi fi-rr-envelope to the "Additional CSS Class(es)" field.
The resulting button will render as:

    ```html
    <button class="jet-form-builder__action-button icon-fi fi-rr-envelope">
        <i class="fi fi-rr-envelope"></i> Submit
    </button>

## Compatibility
WordPress: Version 5.8 or higher.
JetFormBuilder: Version 2.0 or higher.
PHP: Version 7.4 or higher.

## License
This snippet is open-source and licensed under the MIT License.

## Acknowledgements
JetFormBuilder – For providing a robust form-building solution.
Font Awesome, Flaticon Uicons, and others – For the icon libraries.
