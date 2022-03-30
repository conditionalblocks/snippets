# WPML + CONDITIONAL BLOCKS

``` php

/**
 * My newly created function to determine if blocks should show.
 *
 * @return boolean true/false to show the block or not.
 */
function cb_wpml_is_language($expected_lang) {

  // Short language code - en, pl, dk, etc
	$current_lang = apply_filters( 'wpml_current_language', null );

	if ( $current_lang === $expected_lang ) {
		return true;
	}

	return false;
}


/**
 * Add custom functions to be used with PHP Logic conditions.
 *
 * @param array $allowed_functions
 * @return array $allowed_functions
 */
function custom_add_allowed_function_conditional_blocks( $allowed_functions ) {

	array_push( $allowed_functions, 'cb_wpml_is_language' );

	return $allowed_functions;
}
add_filter( 'conditional_blocks_filter_php_logic_functions', 'custom_add_allowed_function_conditional_blocks', 10, 1 );
```
