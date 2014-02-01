---
title: "Casting a string to a number in Sass"
layout: post
comments: true
preview: false
---
<section>
Hey guys! I am currently working on a JSON parser in Sass (yes, you read right) thus I faced an issue I thought unsolvable until now: casting a string into a number in Sass. Needless to say I found a solution! Even better, I found a solution to convert a string into a valid CSS length you can use as a CSS value, in calculations and stuff.

I have to say I am pretty proud with what I have come up with. Not only does it work, but it is also very simple and from what I can tell quite efficient. This may be a bit slower for very large numbers but even there I'm not sure we can feel the difference in compilation time. It also lacks of support for very scientific notation like `e` but that's no big deal for now.
</section>
<section id="core">
## Building the function [#](#core)

As I said, the function is actually simple. It relies on parsing the string character after character in order to map them to actual numbers. Then once you have numbers &mdash; well &mdash; you can do pretty much any thing. Let's start with the skeleton, shall we?

<pre class="language-scss"><code>@function number($string) {
  // Matrices
  $strings: '0' '1' '2' '3' '4' '5' '6' '7' '8' '9';
  $numbers:  0   1   2   3   4   5   6   7   8   9;
  
  // Result
  $result: 0;

  // Looping through all characters
  @for $i from 1 through str-length($string) {
    // Do magic
  }
  
  @return $result;
}</code></pre>

I think you can see where this is going. Now let's have a look at what happens inside the loop:

<pre class="language-scss"><code>@for $i from 1 through str-length($string) {
  $character: str-slice($string, $i, $i);
  $index: index($strings, $character);
    
  @if not $index {
    @warn "Unknown character `#{$character}`.";
    @return false;
  }
      
  $number: nth($numbers, $index);
  $result: $result * 10 + $number;
}</code></pre>

And this is enough to cast any positive integer from a string. But wait! What about negative integers? Plus I told you `number`, not `integer`. Let's continue the journey!
</section>
<section id="negative-numbers">
## Dealing with negative numbers [#](#negative-numbers)

Dealing with negative numbers is very easy: if we spot a dash (`-`) as a first character, then it's a negative number. Thus, all we have to do is to multiply `$result` by `-1` (as soon as `$result` isn't `0`).

<pre class="language-scss"><code>@function number($string) {
// ...
$result: 0;
$minus: false;

@for $i from 1 through str-length($string) {
  // ...
  @if $character == '-' {
    $minus: true;
  }
  
  @else {
    // ...
    $result: $result * 10 + $number;
  }
  
  @return if($minus, $result * -1, $result);
}</code></pre>

As I said, it is pretty straight forward.

</section>
<section id="decimal-dot">
## Dealing with decimal dot [#](#decimal-dot)

Making sure we can convert floats and doubles took me a couple of minutes. I couldn't find a way to deal with numbers once the decimal dot has been found. I always ended up with a completely wrong result until I find a tricky way.

<pre class="language-scss"><code>@function number($string) {
  // ...
  $result: 0;
  $divider: 0;
  
  @for $i from 1 through str-length($string) {
    // ...
    @if $character == '-' {
      // ...
    }
    
    @else if $character == '.' {
      $divider: 1;
    }
    
    @else {
      // ...
      
      // Decimal dot hasn't been found yet
      @if $divider == 0 {
        $result: $result * 10;
      }
      
      // Decimal dot has been found
      @else {
        // Move the decimal dot to the left
        $divider: $divider * 10;
        $number: $number / $divider;
      }
      
      $result: $result + $number;
    }
  }
  
  @return if($minus, $result * -1, $result);
}</code></pre>

Since it can be a little tricky to understand, let's try with a quick example. Here is what happen when we try to cast "13.37" to a number:

1. We set `$divider` and `$result` variables to `0`
2. `"1"` gets found
    1. `$divider` is `0` so `$result` gets multiplied by `10` (still `0`)
    2. `1` gets added to `$result` (now `1`)
3. `"3"` gets found
    1. `$divider` is `0` so `$result` gets multiplied by `10` (now `10`)
    2. `3` gets added to `$result` (now `13`)
4. `"."` gets found
    1. `$divider` is now set to `1`
5. `"3"` gets found
    1. `$divider` is greater than `0` so it gets multiplied by `10` (now `10`)
    2. `3` gets divided by `$divider` (now `0.3`)
    3. `0.3` gets added to `$result` (now `13.3`)
6. `"7"` gets found
    1. `$divider` is greater than `0` so it gets multiplied by `10` (now `100`)
    2. `7` gets divided by `$divider` (now `0.07`)
    3. `0.07` gets added to `$result` (now `13.37`)

</section>
<section id="css-lengths">
## Dealing with CSS lengths [#](#css-lengths)

All we have left is the ability to retrieve the correct unit from the string and returning the length. At first I thought it would be hard to do, but it turned out to be very easy. I moved this to a second function to keep things clean but you could probably merge both functions.

First we need to get the unit as a string. It's basically the string starting from the first not-numeric character. In `"42px"`, it would be `"px"`. We only need to slightly tweak our function to get this.

<pre class="language-scss"><code>@function number($string) {
  // ...
  @for $i from 1 through str-length($string) {
    // ...
    @if $char == '-' {
      // ...
    }
    @else if $char == '.' {
      // ...
    }
    @else {
      @if not $index {
        $result: if($minus, $result * -1, $result);
        @return _length($result, str-slice($string, $i));
      }
      // ...
    }
  }
  // ...
}</code></pre>

If we come to find a character that is neither `-`, nor `.` nor a number, it means we are moving onto the unit. Then we can return the result of the `_length` function.

<pre class="language-scss"><code>@function _length($number, $unit) {
  $strings: 'px' 'cm' 'mm' '%' 'ch' 'pica' 'in' 'em' 'rem' 'pt' 'pc' 'ex' 'vw' 'vh' 'vmin' 'vmax';
  $units:   1px  1cm  1mm  1%  1ch  1pica  1in  1em  1rem  1pt  1pc  1ex  1vw  1vh  1vmin  1vmax;
  $index: index($strings, $unit);
  
  @if not $index {
    @warn "Unknown unit `#{$unit}`.";
    @return false;
  }
  
  @return $number * nth($units, $index);
}</code></pre>

The idea is the same as for the `number` function. We retrieve the string in the `$strings` list in order to map it to an actual CSS length from the `$units` list, then we return the product of `$number` and the length. If the unit doesn't exist, we simply return false.
</section>
<section id="examples">
## Examples [#](#examples)

If you want to play with the code or the function, you can check it on [SassMeister](http://sassmeister.com/gist/8414840). In any case, here are a couple of examples of our awesome little function:

<pre class="language-scss"><code>sass {
  cast: "-15";    // -15
  cast: "-1";     // -1
  cast: "-.5";    // -.5
  cast: "-0";     // 0
  cast: "0";      // 0
  case: ".10";    // 0.1
  cast: "1";      // 1
  cast: "1.5";    // 1.5
  cast: "10.";    // 10
  cast: "12.380"; // 12.38
  cast: "42";     // 42
  cast: "1337";   // 1337
  
  cast: "-10px";  // -10px
  cast: "20em";   // 20em
  cast: "30ch";   // 30ch
  
  cast: "1fail";  // Error
  cast: "string"; // Error
}</code></pre>
</section>
<section id="final-words">
## Final words [#](#final-words)

So guys, what do you think? Pretty cool isn't it? I'd be glad to see what you could be using this for so if you ever come up with a usecase, be sure to share. ;)

Oh by the way if you need to cast a number into a string, it is nothing easier than `$number + unquote("")`. 
</section>
