# wp_php_form_validator
**Wordpress PHP form validator v1.02**

Author: Ganesh Rathinavel

Requirements: PHP 5, Wordpress

URL: [https://github.com/ganeshrvel/wp_php_form_validator](https://github.com/ganeshrvel/wp_php_form_validator)

> Feel free to convert this library into a standalone version or for
> other PHP frameworks by modifying/deleting the WordPress database
> dependencies.


Import the library file:
require_once('form-validator.php')

Execute:

    $vObj = new FormValidator();
    
    $vObj->set_rules( $options_key_1 );
    $vObj->set_rules( $options_key_2 );
    
    #Check 'Example' Section for $options
    
    if ( $vObj->run() == false ) {
       echo "error";
       //dont continue
    } else {
       echo "success";
       //continue
    }
    


----------
Errors:

    /**
     * shows all validation errors
     */
    echo $vObj->show_all_errors();
    
    /**
     * displays single error
     */
    echo $vObj->show_error();
    
    /**
     * param 1: shows 2 error,
     * param2: strips all styles,
     * param3: a 'special character' to seperate multiple errors [Only valid while $strip_styles = true],
     * param4: display "developer only" debugging errors
     */
    
    echo $vObj->show_error( 2,
       true,
       "|",
       true );
    
    /**
     *show the validation error for a specific field
     */
    echo $vObj->show_error_item( 'age1' );
    
    /**
     * returns the 'form field name' in case an error occurs
     */
    echo $vObj->error_field_id;
    
----------

    /**
     * @param $param : form input field name
     * @param bool $trim : trim the output (only for non array (String) inputs)
     * @param bool $isArray : true if an array value is expected from the form input
     * @param bool $sql_escape : escape the input value against sql injections
     *
     * @return null|string
     */
    echo $vObj->post( 'age1', $trim = false, $isArray = false, $sql_escape = false );
    echo $vObj->get( 'age1', $trim = false, $isArray = false, $sql_escape = false );
    echo $vObj->request( 'age1', $trim = false, $isArray = false, $sql_escape = false );
    echo $vObj->files( 'file1', $trim = false, $isArray = false, $sql_escape = false );

----------

    /**
     * pass a custom array value instead of form input parameters; 
     * 'type' won't be valid after $custom_array is set
     * */
    
    $custom_array_validate = array (
       'key1' => 'value1',
       'key2' => 'value2',
    );
    $vObj = new FormValidator( $custom_array_validate );

----------
Example:

    $vObj->set_rules( array (
       'name'                  => 'age1', //HTML form 'field name'; allow_array supported
       'placeholder'           => 'Age 1', //used for displaying errors; allow_array supported
       'type'                  => 'post', //FORM method; allow_array supported
       'required'              => true, //allow_array supported
       'allow_array'           => true, //allow_array: true -> Both Array and String values from the form field are allowed, allow_array: false -> Only String is allowed
       'is_array'              => true, //is_array: true -> Only array values are expected from the form field ; allow_array supported;
       'trim'                  => true, //allow_array supported
       'matches'               => array ( 'The matching placeholder', $vObj->get( 'name2' ) ), //allow_array supported
       'differs'               => array ( 'The differing placeholder', $vObj->get( 'name3' ) ), //allow_array supported
       'is_duplicate'          => array (
          'table_name'        => 'pdg_user_info',//wp db table name
          'column_name'       => 'mobile_no',
          'primary_key'       => 'personal_info_id',
          'primary_key_value' => 3608,
       ),
       'is_unique'             => array ( 'wp_wordpress_user', 'email' ), //allow_array supported
       'custom_error'          => "Some custom error message", //allow_array supported
       'min_length'            => 2, //allow_array supported
       'max_length'            => 5, //allow_array supported
       'exact_length'          => 4, //allow_array supported
       'greater_than'          => 1254,
       'greater_than_equal_to' => 1254,
       'less_than'             => 1245,
       'less_than_equal_to'    => 1245,
       'max_item_limit'        => 5, //Only supported when the form field value is an array
       'min_item_limit'        => 3, //Only supported when the form field value is an array
       'exact_item_limit'      => 4, //Only supported when the form field value is an array
       'unique_items'          => true, //Checks whether there are any duplicated values inside the inputted array; Only supported when the form field value is an array
       'in_list'               => array ( "1" => "First year", 2 => "Second year", 3 => "Third year" ), //check whether any of these values exists; an associative or sequential array supported; allow_array supported;
       'alpha'                 => true,
       'alpha_space'           => true,
       'alpha_numeric'         => true,
       'alpha_numeric_spaces'  => true,
       'alpha_dash'            => true,
       'alpha_numeric_dash'    => true,
       'alpha_dot'             => true,
       'alpha_numeric_dot'     => true,
       'integer'               => true,
       'numeric'               => true, //allow_array supported
       'decimal'               => true,
       'allow_special_chars'   => false,
       'allow_space'           => false,
       'allow_alpha'           => false,
       'allow_number'          => false,
       'valid_url'             => true, //allow_array supported
       'valid_email'           => true, //allow_array supported
       'valid_phone'           => true, //allow_array supported
       'valid_date'            => true,
       'date_range'            => array ( 'min' => '2009-06-17', 'max' => '2009-09-05' ),
       'valid_ip'              => true,
       'sql_escape'            => true, //allow_array supported
       'sanitize_input'        => true, //allow_array supported
       'file_extension'        => array ( 'jpg', 'jpeg', 'png', 'bmp' ), //only supported by files type
       'file_max_size'         => 1000000, //only supported by the 'files' type; in bytes 1000000 = 1 MB
    ) );
    
    $vObj->set_rules( array (
       'name'                  => 'age2',
       'placeholder'           => 'Age 2',
       'type'                  => 'get',
       'required'              => true,
       'trim'                  => true,
       'allow_array'           => false,
       'matches'               => array ( 'Age 1', $vObj->get( 'age1' ) ),
       'differs'               => array ( 'Age 3', $vObj->get( 'age3' ) ),
       'is_unique'             => array ( 'wp_wordpress_user', 'email' ),
       'min_length'            => 2,
       'max_length'            => 5,
       'exact_length'          => 4,
       'greater_than'          => 1254,
       'greater_than_equal_to' => 1254,
       'less_than'             => 1245,
       'less_than_equal_to'    => 1245,
       'in_list'               => array ( 111, 222, 333 ),
       'alpha'                 => true,
       'alpha_space'           => true,
       'alpha_numeric_spaces'  => true,
       'alpha_numeric'         => true,
       'alpha_dash'            => true,
       'alpha_numeric_dash'    => true,
       'alpha_dot'             => true,
       'alpha_numeric_dot'     => true,
       'integer'               => true,
       'numeric'               => true,
       'decimal'               => true,
       'allow_special_chars'   => false,
       'allow_space'           => false,
       'allow_alpha'           => false,
       'allow_number'          => false,
       'valid_url'             => true,
       'valid_email'           => true,
       'valid_phone'           => true,
       'valid_date'            => true,
       'date_range'            => array ( 'min' => '2009-06-17', 'max' => '2009-09-05' ),
       'valid_ip'              => true,
       'sql_escape'            => true,
    ) );
