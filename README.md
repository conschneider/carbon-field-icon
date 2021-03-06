# Carbon Field Icon

[![Packagist](https://img.shields.io/packagist/vpre/htmlburger/carbon-field-icon.svg?style=flat-square&colorB=0366d6)](https://packagist.org/packages/htmlburger/carbon-field-icon)

Provides the ability to select an icon or a glyph.

## Supported glyphs

- Font Awesome (v5.8.1)
- Dashicons
- Custom

## Usage

Font Awesome only (default):

```php
Field::make( 'icon', 'social_site_icon', __( 'Icon', 'crb' ) ),
```

Dashicons only:

```php
Field::make( 'icon', 'social_site_icon', __( 'Icon', 'crb' ) )
	->add_dashicons_options(),
```

Dashicons and Font Awesome:

```php
Field::make( 'icon', 'social_site_icon', __( 'Icon', 'crb' ) )
	->add_dashicons_options()
	->add_fontawesome_options(),
```

Custom icon list:

```php
Field::make( 'icon', 'social_site_icon', __( 'Icon', 'crb' ) )
	->set_options( array(
		// Minimal settings:
		'my-custom-icon-1' => array(
			'name' => __( 'My Custom Icon 1' ),
			'icon' => get_template_directory() . '/icons/my-custom-icon-1.svg',
		),

		// Full settings:
		'my-custom-icon-2' => array(
			'name'         => __( 'My Custom Icon 2' ),
			'icon'         => get_template_directory() . '/icons/my-custom-icon-2.svg',
			'id'           => 'my-custom-icon-2',
			'class'        => 'my-custom-prefix-class',
			'search_terms' => array( 'shop', 'checkout', 'product' ),
		),
	) ),
```

Custom icon list (using providers):

```php
class Custom_Icon_Provider implements Carbon_Field_Icon\Providers\Icon_Provider_Interface {
	public function parse_options() {
		return array(
			// Minimal settings:
			'custom-icon-1' => array(
				'name' => __( 'Custom Icon 1' ),
				'icon' => get_template_directory() . '/icons/custom-icon-1.svg',
			),

			// Full settings:
			'custom-icon-2' => array(
				'name'         => __( 'Custom Icon 2' ),
				'icon'         => get_template_directory() . '/icons/custom-icon-2.svg',
				'id'           => 'custom-icon-2',
				'class'        => 'custom-prefix-class',
				'search_terms' => array( 'shop', 'checkout', 'product' ),
			),
		);
	}
}

add_action( 'carbon_fields_icon_field_loaded', 'crb_register_custom_icon_field_provider' );
function crb_register_custom_icon_field_provider() {
	$provider_id = 'custom';

	\Carbon_Fields\Carbon_Fields::instance()->ioc['icon_field_providers'][ $provider_id ] = function( $container ) {
		return new Custom_Icon_Provider();
	};

	\Carbon_Field_Icon\Icon_Field::add_provider( [ $provider_id ] );
}

Container::make( 'theme_options', __( 'Theme Options', 'crb' ) )
	->set_page_file( 'crbn-theme-options.php' )
	->add_fields( array(
		Field::make( 'icon', 'crb_custom_icon', __( 'Choose Custom Icon', 'crb' ) )
			->add_provider_options( 'custom' ),
	) );
```

## Added Support for Icomoon

* Create your font files with Icomoon.
* Font files are located in `build/fonts`.
* CSS path `build/icomoon.css`
* New provider for Icomoon located at: `core/Providers/Icomoon_Provider.php`
  * The provider also loads the `icomoon.css`
* `data/icomoon.json` provides the icon list.
* Usage: `->add_icomoon_options()`

If you want to add another provider, search the code for `icomoon`. That should give you all necessary touchpoints.
