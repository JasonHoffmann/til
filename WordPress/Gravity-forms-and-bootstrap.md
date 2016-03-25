#title:  Gravity forms and bootstrap
##date:  2016-03-25 15:02:43
##sub-folder:  WordPress

Getting Gravity Forms to output Bootstrap classes can be a bit difficult, but can basically broken down into two steps. The first is to add a "form-control" class to the wrapper of each input.

```php
add_filter( 'gform_field_container', 'add_bootstrap_container_class', 10, 6 );
function add_bootstrap_container_class( $field_container, $field, $form, $css_class, $style, $field_content ) {
  return '<li class="' . $css_class . ' form-group">{FIELD_CONTENT}</li>';
}
```

The next step, if using SASS, is just to ensure the proper classes are extended:

```sass
.gform_fields {
	padding: 0;
	list-style-type: none;

	ul {
		padding: 0;
		list-style-type: none;
	}

	input, select {
		@extend .form-control;
	}
}

.gfield_description {
	@extend .alert;
}

.validation_error {
	@extend .alert;
	@extend .alert-danger;
}

.validation_message {
	@extend .alert-warning;
	margin-top: -1em;
	margin-bottom: 2em;
}

.gform_button {
	@extend .btn;
	color: white;
}

.gfield_required {
	color: $alert-danger-text;
	margin-left: 2px;
}

.ginput_container {
	margin-bottom: 25px;
	position: relative;
}
```