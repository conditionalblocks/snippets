## Custom User Agent With Conditional Blocks Pro

This snippet is an example of checking the User Agent in WordPress with Conditional Blocks Pro.

For more information about the PHP logic conditions check https://conditionalblocks.com/docs/conditions/php-logic/

``` php
/**
 * Check if the current user agent includes FootyLingoApp.
 *
 * @return bool True if the user agent is FootyLingoApp, false otherwise.
 *
 * @author Morgan from conditionalblocks.com
 */
function is_footy_lingo_app() {
    if (isset($_SERVER['HTTP_USER_AGENT'])) {
        $user_agent = $_SERVER['HTTP_USER_AGENT'];
        if (strpos($user_agent, 'FootyLingoApp') !== false) {
            return true;
        }
    }
    return false;
}

/**
 * Register functions to be used with PHP Logic condition in Conditional Blocks Pro.
 *
 * @param array $allowed_functions
 * @return array $allowed_functions
 */
function custom_allowed_functions_conditional_blocks( $allowed_functions ) {

	array_push( $allowed_functions, 'is_footy_lingo_app' );

	return $allowed_functions;
}
add_filter( 'conditional_blocks_filter_php_logic_functions', 'custom_allowed_functions_conditional_blocks', 10, 1 );
```
