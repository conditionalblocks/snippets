# Change if Conditional Blocks should be visbible in the WordPress Editor.

Available to test in 3.0.4-beta1.

```
/**
 * Determines if conditional blocks should be visible in the editor based on user roles.
 *
 * @param bool $should_be_visible The initial (true) visibility status of Conditional Blocks in the WordPress Editor.
 * @return bool Returns true if the current user has the 'editor' or 'administrator' role, otherwise false.
 */
function custom_conditional_blocks_visible_in_editor($should_be_visible) {
	
	// Check if the current user has specific user roles.
	$user = wp_get_current_user();
	$allowed_roles = array('editor', 'administrator');

	if (array_intersect($allowed_roles, $user->roles)) {
		return true;
	}

	return false;
}

add_filter('conditional_blocks_visible_in_editor', 'custom_conditional_blocks_visible_in_editor', 10 , 1 );
```
