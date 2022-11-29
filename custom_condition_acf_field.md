# Custom Condition to check a ACF Field

This is an example snippet that registers a new Condition within the Conditional Blocks Pro plugin.

How to register conditions: https://conditionalblocks.com/docs/developer/custom-conditions/

``` php
/**
 * Register a new category in the Condition Builder.
 */
function example_condition_categories( $categories ) {

	$categories[] = array(
		'value' => 'example_custom_acf',
		'label' => 'ACF (Custom)',
	);

	return $categories;

}
add_filter( 'conditional_blocks_register_condition_categories', 'example_condition_categories', 10, 1 );


 /**
  * Register Conditions within group.
  */
function example_conditions( $conditions ) {

	// Add a new condition to your group.
	$conditions[] = array(
		'type' => 'custom_acf_user_is_architect', // Important: The type identities the condition and should NOT be changed.
		'label' => 'Current user is architect',
		'description' => 'Only display the select block if the current user ID appears in the get_field("architect")', // Appears above any fields.
		'category' => 'example_custom_acf', // A category the condition will appear in.
	);


	return $conditions;
}
add_filter( 'conditional_blocks_register_condition_types', 'example_conditions', 10, 1 );


/**
 * Check for the Condition Type: my_first_custom_condition
 *
 * NOTE: defaults are not translated from above if the user doesn't interact with the React fields.
 *
 * @param bool  $should_render if condition passed validation.
 * @param array $condition contains the configured conditions with keys/values.
 * @return bool $should_block_render - defaults to false.
 */
function example_my_first_custom_check( $should_block_render, $condition ) {


	// Make sure ACF functions are available. 
	if (!function_exists('get_field')) {
		return $should_block_render;
	}
	
	// Make sure we have a current user.
	$current_user_id = get_current_user_id();
	
	if (!$current_user_id) {
		return $should_block_render;
	}
	
	$users = get_field('architect', '');
	
	foreach($users as $user){
   
   	 $user_id = !empty($user['ID']) ? (int) $user['ID'] : false;
		
		if ($current_user_id === $user_id){
			$should_block_render = true;
			break;
		}
	}

	return $should_block_render;
}

add_filter( 'conditional_blocks_register_check_custom_acf_user_is_architect', 'example_my_first_custom_check', 10, 2 );

```
