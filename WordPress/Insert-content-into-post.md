#title:  Insert content into post
##date:  2016-03-15 16:27:02
##sub-folder:  WordPress

If you want to insert some text about halfway through a piece of content, you can use the_content filter and some array mutations to accomplish this. I'm not sure if this is the most effective way to go about it, but it seems to work pretty well for simple cases where you just need to pass a string.

I grabbed most of this from [WP Beginner](http://www.wpbeginner.com/wp-tutorials/how-to-insert-ads-within-your-post-content-in-wordpress/)

This is the function that actually does the insertion by breaking up paragraphs, and then inserting some additional content in there.

```php
/**
* Add an insertion string 3 paragraphs into passed content
* 
* @param str $insertion The string to insert in the paragraphs
* @param str $content The full string of content
* 
* @return str $paragraphs
*/
function insert_into_middle_of_content( $insertion, $content ) {
	$close_p = '</p>';
	$paragraphs = explode( $close_p, $content );
	foreach( $paragraphs as $index => $paragraph ) {
		if( trim( $paragraph ) ) {
			$paragraphs[$index] .= $close_p;
		}

		if( $index === 1 ) {
			$paragraphs[$index] .= $insertion;
		}
	}

	return implode( '', $paragraphs );
}
```
Then this applies the above function to the content if we are on a single post, and not in the admin.

```php
/**
 * Apply the filter to insert content
 * 
 * @param str $content Content of a post
 */
 function insert_callout_into_content( $content ) {

 	$most_recent_giveaway = new WP_Query( array( 
 		'post_type' => 'giveaways',
 		'posts_per_page' => 1
 		) );
 	if( $most_recent_giveaway->have_posts() ) {
 		while( $most_recent_giveaway->have_posts() ) {
 			$most_recent_giveaway->the_post(); 
 			$isbn = get_field( 'isbn' );
 			$book = new RandomHouse;
 			$book = $book->book( array( 'id' => $isbn ) );
 			$blurb = get_field('giveaways_blurb');
 		}
 	}

 	ob_start();
 	render_template( 'giveaway-callout.php', array( 'book' => $book, 'blurb' => $blurb ) );
 	$code = ob_get_clean();

 	if( is_single() && !is_admin() ) {
 		return insert_into_middle_of_content( $code, $content );
 	}

 	return $content;
 }
 add_filter( 'the_content', 'insert_callout_into_content' );
 ```