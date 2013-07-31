---
layout: post
title: "Array to human readable list"
date: 2013-07-25 22:54
comments: false
categories: [gist, helper, php]
tags: [prettify]
published: false
---
As a full-stack webapplication developver I also do a lot of work on the front-end. Every once in a while I stumble upon printing out an array of data where I don't know the size. It could be empty, there could be 4 items. All I know is regardless the contents, it needs to be prettified for the humans. This means that the data array needs to be turned into a listing. Let's define some data:

<!-- more -->

{% codeblock %} 
$data = array( 
	'mono' 	=> array( 'foo' ),
	'bi' 	=> array( 'foo', 'bar' ),
    'multi' => array( 'foo', 'bar', 'baz' )
);
{% endcodeblock %}

Given these example datasets, when these represent options they should be printed out as

- _mono_: foo
- _bi_: foo or bar
- _multi_: foo, bar or baz

and in case of a listing items **or** should be substituted with **and**.

There are so many ways I could solve this problem and my first approach was going with a decision tree or switch statement. I could simply check for the length and use any form of formatting the string data. Either using sprintf or manually extracting the last two items before imploding the rest.

{% codeblock %} 
function( $input, $last = ' or ', $glue = ', ' ) {
	$len = count( $input );
	if ( $len === 0 )
		return '';
	if ( $len === 1 )
		return $input[0];
	if ( $len === 2 )
		return sprintf( '%s%s%s', $input[0], $last, $last[1] );
	return sprintf( '%5$s%1$s%4$s%2$s%3$s', $glue, $last, array_pop( $input ), array_pop( $input ), implode( $input, $glue ) );  
}
{% endcodeblock %}

{% gist 4514697 %}