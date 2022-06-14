# Conditional Blocks + WooCommerce Detect Products Purchased on Thank You Page

```php
/**
 * My newly created function to determine if blocks should show.
 *
 * @return boolean true/false to show the block or not.
 */
function custom_order_recieved_and_includes( $expected_product_id ) {

	if ( ! is_wc_endpoint_url( 'order-received' ) ) {
		return false;
	}

	global $wp;

	$order_id  = absint( $wp->query_vars['order-received'] );

	if ( empty( $order_id ) || $order_id == 0 ) {
		return false;
	}

	$order = wc_get_order( $order_id );

	$items = $order->get_items();

	foreach ( $items as $item_id => $item ) {
		$product_id = $item->get_variation_id() ? $item->get_variation_id() : $item->get_product_id();

		if ( $product_id === (int) $expected_product_id ) {
			return true;
		}
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

	// Add new conditionals functions ot use with CB.
	array_push( $allowed_functions, 'custom_order_recieved_and_includes' );

	return $allowed_functions;
}
add_filter( 'conditional_blocks_filter_php_logic_functions', 'custom_add_allowed_function_conditional_blocks', 10, 1 );

```
