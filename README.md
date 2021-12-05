# DLM THEME PRO

Sample theme that utilizes the Digital License Manager PRO theme activator and update server.


## Integration

Digital License Manager PRO update server is pretty simple to integrate in any theme, this is just a step by step example:

1. Download the library from [GitHub](https://github.com/ideologix/dlm-wp-updater)
2. Copy it in your theme somewhere, eg. inc/updater/dlm-wp-updater
3. Create init.php in inc/updater/init.php and follow the integration guide below

#### init.php file

```php
<?php

require_once rtrim( dirname( __FILE__ ), '/' ) . '/dlm-wp-updater/autoload.php';

use IdeoLogix\DigitalLicenseManagerUpdaterWP\Main;

/**
 * Class DLM_Theme_Activation
 */
class Theme_Name_Updates_Activator {

	/**
	 * The instance
	 * @var Main
	 */
	protected $instance;

	/**
	 * DLM_Theme_Activation constructor.
	 */
	public function __construct() {

		try {
			$this->instance = new Main( array(
				'id'              => 2,
				'name'            => 'DLM THEME PRO', // The theme name, same as style.css
				'context'         => 'theme', // Must specify this if it is theme.
				'file'            => get_template_directory(), // Tip: the theme root directory
				'basename'        => 'dlm-theme-pro', // Tip: the theme folder name
				'version'         => THEME_NAME_VERSION, // Tip: Define this in functions.php and increase with every release, this is important because it checks for updates against this version.
				'url_settings'    => 'https://url-to-your-plugin/settings-page',
				'url_purchase'    => 'https://url-to-your-website/purchase-page',
				'consumer_key'    => 'ck_77633bPP12b56efc951856e645c65dd70dddd111',
				'consumer_secret' => 'cs_b9991aXX1c43b8b8681714498f5b80c2e002c020',
				'api_url'         => 'http://dlm.test/wp-json/dlm/v1/', // Your Digital License Manager PRO site server
				'prefix'          => 'dlm',
			) );

		} catch ( \Exception $e ) {
			error_log( 'Error: ' . $e->getMessage() );
		}

		add_action( 'admin_menu', array( $this, 'admin_menu' ) );

	}

	/**
	 * Registers the theme activation page
	 */
	public function admin_menu() {
		add_submenu_page(
			'themes.php',
			__( 'Theme Activation', 'dlm-theme-pro' ),
			__( 'Theme Activation', 'dlm-theme-pro' ),
			'manage_options',
			'dlm-theme-pro-activation',
			array( $this, 'render_activation_form' )
		);
	}

	/**
	 * Render the activation form
	 */
	public function render_activation_form() {

		echo '<div class="wrap">';
		if ( empty( $this->instance ) ) {
			echo '<h2>Argh!</h2>';
			echo "<p>Theme Activator not initialized, yet.</p>";
		} else {
			echo '<h2>Theme Activation</h2>';
			echo '<div class="theme-activator">';
			$this->instance->getActivator()->renderActivationForm();
			echo '</div>';
			echo '<style>.theme-activator {background-color: #fff; padding: 10px; max-width: 50%; margin-top: 10px;}'; // Do your styling. This is just a plain form.
		}

		echo '</div>';

	}

}

new Theme_Name_Updates_Activator();


```

