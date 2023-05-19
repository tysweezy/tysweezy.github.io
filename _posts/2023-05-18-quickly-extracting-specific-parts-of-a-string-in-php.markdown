---
layout: post
title:  "Quickly Extracting Specific Parts of a String in PHP"
date:   2023-05-18  14:57:17 -0700
categories: software engineering
author: Tyler Souza
share: true
---


In the current project I am working on, there are certain characters within a string that identify a part that get decoded before they enter the database. In this partial of the string, it used to contain a dash `DDA-7.5` like so. They are both identifiers that represent different things. I can `explode` this out the split the string by the dash to get one part of the string for the model name at the beginning and the capacity number at the end. Done!

However, requirements changed and now the structure is required to be in this format, `DDA60`. What do we do now? [str_split](https://www.php.net/manual/en/function.str-split.php) to the rescue!


## Lets Do This!

Here's how we can grab the model name at the front part of the text. 

```php
function parseModel($modelKey) {
	$modelArr = str_split($modelKey);
	$model = array_slice($modelArray, 0, 3);


    return implode($model);
}

```


Sweet! We can split the string into an array with `str_split`. Then, we can slice the first three characters with `array_slice`. 

What about the last characters though? We can update the function to account for this. 

```php
function parseModelInfo($modelKey, $type) {
	$modelArr = str_split($modelKey);

	if ( $type == 'capacity' ) {
		$cap = array_slice($modelArray, 3);
		 return implode($cap);
	}

	$model = array_slice($modelArray, 0, 3);


    return implode($model);
}

```

The last parts after the 2nd index represent the "capacity." We add in a parameter to check for this. If so, we [array_slice](https://www.php.net/manual/en/function.array-slice.php) from the 3rd index onward. Not if the third parameter is null, it will grab the rest of the values unless specified.  Easy enough!


## A Note on Implode

See how we didn't specify a delimiter in the `implode` returns? That is because that parameter is defaulted to an empty string. You can check out the docs [here](https://www.php.net/manual/en/function.implode.php)!