# A simple tutorial on RegEx: Matching Hex codes

Hex codes are important to find the correct color you want for a project, but it's almost impossible to verify a code if you use JavaScript alone. JavaScript is too methodic to accurately verify a hex code, and that's where RegEx comes in. With RegEx, users can verify anything from a phone number to a full URL with great accuracy, but for this tutorial, RegEx will be used to verify a color hex code.

## Summary

RegEx, or Regular Experssion, are a set of special characters within two forward slashes (`/`). This tutorial will create a line of code to verify a hex code, and figure out what it does to help others use RegEx for more complex use cases. An example of such a use case would be this:

``` /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/ ```

Note that this is used to match a URL, but this tutorial will not be used for a URL.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

As stated before in the [Summary](#summary), a RegEx is encased within two forward slashes. An example of this is `/RegEx/`, and is required in order for RegEx code to run. If left outside of forward slashes, RegEx will throw syntax errors because most of it's symbols aren't used to code in standard JavaScript.

Note that there are two ways of creating an expression: **RegEx** and **RegExp**. Here are two examples of both expression types:

RegEx:

``` const re = /[a-zA-Z]+[1-9]/m ```

RegExp:

``` const rep = new RegExp("[a-zA-Z]+[1-9]", "m"); ```

Both cases do the exact same thing, but are coded in different styles. While RegEx has better preformance, RegExp has better compilation, but both can be used interchangeably. There is also more that RegExp and RegEx can do that is not listed in this tutorial.

Because all RegEx requires forward slashes, and RegExp is no being used, the hex code matcher should follow the RegEx example. For now, underscores (`_`) will fill in the blanks.

``` const hcMatcher = /__/ ```

### Anchors

There are two anchor symbols in RegEx: the power symbol (`^`) and the dollar sign (`$`). These two are the base of RegEx infastructure, as they are used to contain the characters used. 

The `^` anchor is used as a starting point for RegEx, and is almost always placed at the start of a RegEx. It is also used to dicern specific words or characters. An example would be `^This`, which would confirm the lines `This` and `This plate`, but would reject `this` or `this person`.

The `$` anchor is similar to the `^` anchor, as it is user as the end point for RegEx, and it's almost always placed at the end of a RegEx. An example of both anchors being used is `/^chatbox$/`, where it will confirm any line that uses exactly `chatbox`, and refuse anything else. That's because it doesn't have any additional expressions or symbols to modify it for realistic use.

Something to note is that some, albeit few, types of RegEx code do not need anchors. A "well-known" example of this is with `/ab+c/`, which doesn't use anchors due to it's shortness.

Because almost all RegEx uses an anchor, the hex code matcher should have one as well, and since anchors alone can dicern specific characters, a hashtag symbol (`#`) can be put right at the beinging of the code, since all hex codes usually start with a hashtag. 

``` const hcMatcher = /^#__$/ ```

### Quantifiers

Quantifiers are used to limit the amount of times a pattern is checked to a minimum and maximum value. This is usually used for things like passwords or codes, which usually have a maximum character limit. Without a quantifier to limit a RegEx, a password or code could be anywhere between one character, or one thousand characters long. With a quantifier however, a RegEx can reject a pattern that is below the minimum limit or above the maximum limit.

There are three quantifier symbols, the asterisk symbol (`*`), the plus symbol (`+`), and the question mark symbol (`?`), which are all **broad quantifiers**. The `*` quantifier checks the pattern zero or more times, meaning it will confirm practically any pattern as long as it matches the specified [bracket expressions](#bracket-expressions). The `+` quantifier checks the pattern one or more times, meaning it will reject 0-lined or blank patterns. The `?` quantifier checks the pattern either zero or one times, meaning it will reject and 2-lined or above pattern. These symbols can be used to broadly match a pattern as many times as needed, with the exception of the `?` quantifier, which is only useful to match specific characters or patterns, like `@`, `https`, or `\`.

To match a pattern a specific number of times, use curly brackets (`{}`), which is a **limiting quantifier**. Curly brackets require a numeral value to be added in to set the limit. Using `{n}` will check for exactly "n" patterns, so the line `{3}` will match exactly 3-lined patterns and reject anything else. Using `{n,}` will check for at *least* "n" patterns, so the line `{2,}` will match any 2-lined pattern, but can also match a 3-lined, 4-lined, or higher pattern, but will reject any 1-lined or 0-lined pattern. Using `{n,x}` will check for a minimum of "n" patterns, and a maximum of "x" patterns, so the line `{4-6}` will match any patterns that are between 4 lines and 6 lines, and will reject anything that is above or below that threshold.

An example of a limiting quantifier is `{5,12}`, which will confirm any pattern that has a minimum of 5 characters and a maximum of 12, so patterns like `ed43wer`, `Hoarfrost`, `wellness`, and `fromTwelve12` would be confirmed, while patterns like `qual`, `w15c`, and `aboveaboveabove` will be rejected. An example of broad quantifiers is `8+re*`, which will confirm any pattern that has one `8` or as many `8`s, as long as it doesn't come before an `r`. The `e` does not need to be in the pattern, and if it is, there can be as many as their possibly can, but only if it's after the `r`. Patterns that are confirmed by this broad RegEx is `8888r`, `8re`, and `88reeeeeeee`, while patterns that are rejected are `reeee`, `8888er`, `reeeee8`, and `88888eee`.

Hex codes are always in threes and sixs, so the matcher should be coded with limiting quantifiers.

``` const hcMatcher = /^#__{3,6}__$/ ```

### OR Operator

OR operators, as the name suggest, searches for a match or matches in either expression that is available within the operator. This operator is depicted as a pipe symbol (`|`), but is also found passively in [bracket expressions](#bracket-expressions). An example of a line of code that uses OR operators is `/^{4}|{11}|far$/`. This code will confirm any 4-lined *or* 11-lined pattern, and will also confirm any pattern that is exactly `far`, despite it being a 3-lined pattern. Examples of confirmed patterns are `WILD`, `swee`, `2g45`,`wrldwideWeb`, and `far`. Examples of rejected patterns are `wayfinder`, `WoW`, `masterpieces`, and `vast`.

As there are no 4-lined or 5-lined patterns in hex codes, an OR operator can be used to polish our code, so it can look for *exactly* 3-lined and 6-lined patterns.

``` const hcMatcher = /^#__{3}|__{6}__$/ ```

### Character Classes

Character classes are sets of characters that are easily defined in a RegEx, and are used inside [bracket expressions](#bracket-expressions). They use backslashes (`/`), which give specific characters special properties to allow them to mimic multiple sets of normal characters. There are even some that checks for whitespaces or binary. Here are some examples:

- `\.`: Matches anything that isn't a linebreak (or `\n`). An example of this would be `[a-z\.]`.
- `\d`: Matches any Arabic numeral. This is functionally identical as `[0-9]`. An example of this would be `[a-lG-P\d]`.
- `\t`: Matches a horizontal tab. An example of this would be `[\t]`, which would match `    if (keyPressed('downarrow') && keyPressed('rightarrow')) {` because there is a horizontal tab at the start.

Note that character classes can be inversed with capitalized letters. `\.` is an exception, since a period cannot be capitalized.

The hex code matcher doesn't actually need any character classes, but if they were to be added they would replace the normal array of numbers.

``` const hcMatcher = /^#__\d__{3}|__\d__{6}__$/ ```

### Flags

Flags are a special class, as they exist *outside* the main RegEx expression as a modifier to it. Back in the [RegEx Components](#regex-components) sections, there is a flag located inside the example:

``` const re = /[a-zA-Z]+[1-9]/m ```

That "m" in the code is the flag that modifies the main RegEx expression, and there are 5 more flags: `i`, `u`, `s`, `g`, and `y`. All of them have their uses, but `i`, `g`, and `m` are the most commonly used. There uses are as follows:

- `\g`: Searches for all possible matches within a pattern. Without `\g`, RegEx will only look for the first match.
- `\i`: Removes the case-sensitive restrictions, which make letters like `e` and `E` functionally the same.
- `\m`: Searches across multiple lines instead of being contained within a string's length.

The matcher will work without a flag, but a `/g` one would be very appreciated.

``` const hcMatcher = /^#__{3}|__{6}__$/g ```

A `/i` would also be useful, as a hex code does not care if a letter is upper- or lowercased.

``` const hcMatcher = /^#__{3}|__{6}__$/i ```

### Grouping and Capturing

Grouping a RegEx is used to match multiple parts of a string to see if they match their respective requirements. To group into a string, use parentheses (`()`) inbetween what to look for specifically. This is mainly used when writing a RegEx that uses special characters like `@`, `&`, or `#`, that need a higher priority of being checked.

An example of this is `/^(rec)=[\w+]$/i`, which checks for the pattern `rec` semi-exactly (the `/i` flag makes the match more lenient), then checks for the `=` symbol, then checks for any word that uses only letters and numbers. Confirmed examples would be `rec=This`, `reC=r4C3`, and `REC=ROOM`. Rejected examples would be `Rend=rec`, `rec=D3LT@`, and `rec1=rec1`.

Capturing is partially as simple as grouping. Bascially, a group is by default, *captured* and saved for later use, but it's possible to not capture a group if it would only be used once, by using the code `?:` at the start of a group. It should be noted that capturing groups allows modifying them later in the algorithm and are better used in smaller, more simpler expressions, while non-capturing groups are more useful in longer and complex expressions, due to this limit some engines put on captured groups in an expression.

The matcher is small, so it doesn't need to worry about the limit. However, it does have a special character that would need it's own specific check, so excluding it from the hex code value would be needed.

``` const hcMatcher = /^#__(__{3}|__{6})$/ ```

### Bracket Expressions

Bracket expressions are the basis of a RegEx, as they are used as a legend or key of sorts to find the correct characters necessary for a match. Bracket expressions are created using, as the name implies, brackets (`[]`) and are necessary for many of the classes and operators in this tutorial. An example of a bracket expression is `/^[tuvw]$/`, which will match anything that has the letters, `t`, `u`, `v`, or `w`. Using a hyphen (`-`), this code can be polished to `/^[t-w]$/`, which is functionally identical to `/^[tuvw]$/`. Examples of confirmed matches with this code is `Triton`, `umbra` and `v3r3`, while rejected matches would be `IOweHim`, `splash`, and `1457888`.

It's possible to include special characters like the underscore (`_`) or hyphen, but only after the alphanumeric characters. An example of this would be `/^[a-zA-Z0-9_-]$/`, which will confirm `cheesy`, `Gallio-Rargan`, and `this_db`, and reject `this@gmail.com` and `52%`.

Bracket expressions are naturally **positive**, will means that it will include anything inside them. However, adding a power symbol (`^`) to the start of the bracket expression inverses it, creating a **negative bracket expression**, which excludes anything. An example of a negative bracket expression is `/^[^iou]$/`, which will reject anything that has the letters `i`, `o`, and `u`, but confirm anything else.

As hex codes are listed with the letters `a` to `f`, and utilize the numbers `0` through `9`, adding a bracket expression to both side of the matcher will allow the code to confirm any matches that coralate to a hex color, while rejecting anything that tried to fake a hex code.

``` const hcMatcher = /^#__([a-f0-9]{3}|[a-f0-9]{6})$/ ```

Alternatively, using the character class `\d`, the numeral character can be replaced to make the code more polished.

``` const hcMatcher = /^#__([a-f\d]{3}|[a-f\d]{6})$/ ```

### Greedy and Lazy Match

Greedy and lazy matches are simple to understand, and it goes back to [quantifiers](#quantifiers). Basically, all quantifiers are **greedy**, meaning they check for as many matches as possible. The most notable ones are the `*` and `+` quantifiers, which checks for 0-many matches and for 1-many matches as possible respectively. The only execption is the `?` quantifier, which is the only **lazy** quantifier in RegEx. Lazy quantifiers check for either 0 or 1 matches, and sends back a rejection if it detect more then 1 match.

Since a hex code will not work if there are two hashtag symbols (`#`), putting the lazy quantifier, `?`, keeps the matcher from confirming a hex code with double hashtags.

``` const hcMatcher = /^#?([a-f0-9]{3}|[a-f0-9]{6})$/ ```

## Extras

This is the end of the actual tutorial and making a hex code matcher and understanding the code that makes it possible. This is what should be created from this:

``` const hcMatcher = /^#?([a-f0-9]{3}|[a-f0-9]{6})$/ ```

Of course we can optimize this more but adding character classes and flags.

``` const hcMatcher = /^#?([a-f\d]{3}|[a-f\d]{6})$/i ```

Anything from here will be optional and not necessary to know most of the basics of RegEx.

### Boundaries

l

### Back-references

l

### Look-ahead and Look-behind

l

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)

I am Matthew Earls, the writer of this tutorial (even though it could be argued to be an article). This was an interesting experience writing this, but I hope you, the reader, enjoyed this to at least a slight degree (I have low expectations for many things). If a link to my GitHub profile in needed, [here it is.](https://github.com/Nightfeller).
