# Theme Development

Gutenmate supports integration from theme to have a better behavior and reduce work for theme developer. This section is for developers who have knowledge of coding and WordPress theme development.

## Recipe summary
A custom field set for inputting a recipe summary (used for creating a recipe post). Use the below code to enable it.

```php
add_theme_support( 'gutenmate-recipe-summary' );
```

## Place summary
A custom field set for inputting a place summary (used for creating a post about a hotel, restaurant, etc. ). Use the below code to enable it.

```php
add_theme_support( 'gutenmate-place-summary' );
```

## Event summary
A custom field set for inputting an event summary (used for creating a post about a seminar,, etc. ). Use the below code to enable it.

```php
add_theme_support( 'gutenmate-event-summary' );
```

## Predefined image sizes

Gutenmate setting page has an interface to manage the image size. It's easy for user to disable or change the image size. To use this feature, Theme must define an image size and it will be registered by `add_image_size()`. Use the below code to add a predefined sizes.

```php
add_theme_support( 'gutenmate-image-sizes', [
	['enable' => true, 'name' => 'theme_thumbnail', 'width' => 60, 'height' => 60, 'crop' => false],
	['enable' => true, 'name' => 'theme_small', 'width' => 256, 'height' => 170, 'crop' => true, 'cropX' => 'left', 'cropY' => 'center'],
	['enable' => true, 'name' => 'theme_large', 'width' => 538, 'height' => 717, 'crop' => true, 'cropX' => 'center', 'cropY' => 'top'],
] );
```

## Predefined spacing

Theme can register a predefined spacing for selecting in all spacing options. Use the below code to add a predefined sizes.

```php
add_theme_support( 'gutenmate-spacing', [
	[
		'name' => esc_html__( 'Zero', 'theme' ),
		'slug' => 'zero',
		'size' => '0px',
	],
	[
		'name' => esc_html__( '4XS', 'theme' ),
		'slug' => 'small',
		'size' => '4px',
	],
] );
```

## Predefined font family

Theme can register a font families for using entire size. The `fontFamily` can be a name of google font. It will be attached to the page on rendering.

```php
add_theme_support( 'gutenmate-font-families', [
	[
		'name'              => esc_html__( 'Heading', 'theme' ),
		'slug'              => 'heading',
		'fontFamily'        => 'Roboto Slab',
		'preloadFontWeight' => '300,400,500,600,700,800,900',
	],
	[
		'name'              => esc_html__( 'Body', 'theme' ),
		'slug'              => 'body',
		'fontFamily'        => 'Inter',
		'preloadFontWeight' => '400,700,800,900',
	],
] );
```
The registered font family will be generate a css variable as follows. You can use this variable to assign into any element.

```css
body {
	--gtm--font-family--heading: "Roboto Slab";
    --gtm--font-family--body: "Inter";
}
```

## Predefined typography

Theme can register a predefined typography preset for selecting in all typography options.

```php
add_theme_support( 'gutenmate-typography', [
	[
		'name'           => esc_html__( 'Heading 1', 'theme' ),
		'slug'           => 'heading-1',
		'font-family'    => 'var(--gtm--font-family--heading)',
		'font-weight'    => '600',
		'font-size'      => 'var(--wp--preset--font-size--4-xl)',
		'line-height'    => '1.15',
		'letter-spacing' => '0px',
		'text-transform' => 'none',
	],
	[
			'name'        => esc_html__( 'Main menu', 'theme' ),
			'slug'        => 'main-menu',
			'font-family' => 'var(--gtm--font-family--body)',
			'font-weight' => '600',
			'font-size'   => [
				'sm' => '16px',
				'md' => '18px',
				'lg' => '22px',
			],
			'line-height' => [
				'sm' => '1.1',
				'md' => '1.2',
				'lg' => '1.3',
			],
			'letter-spacing' => [
				'sm' => '0px',
				'md' => '1px',
				'lg' => '2px',
			],
			'text-transform' => 'uppercase',
		],
] );
```

## Registering a custom icon set

```php
add_action( 'after_setup_theme', 'example_register_theme_icon_set' );
function example_register_theme_icon_set() {
	wp_register_style( 'my-basic-icons-handle', get_theme_file_uri( '/assets/my-basic-icons/my-basic-icon.css' ), [], GTMT_VERSION );

	if ( class_exists( 'GTM_Icon_Set' ) ) {
		GTM_Icon_Set::register( [
			"slug"          => "my-basic-icons",
			"name"          => esc_html__( "Theme Basic Icons", 'theme' ),
			"description"   => "Theme's basic icons",
			"link"          => esc_url( 'https://example.com/icon-list'),
			"enqueue_style" => "my-basic-icons-handle",
			"meta"          => get_template_directory() . '/assets/my-basic-icons/my-basic-icons.txt',
		] );
	}

	// Activate theme's icon set
	add_theme_support( 'gutenmate-icon-sets', ['my-basic-icons'] );
}
```

For `meta` attribute that require a url to a meta file, The file content must contain a class name in each line. Example:

```
my-basic-icon-arrow
my-basic-icon-bag
my-basic-icon-image
my-basic-icon-video
```