# **e/grep:**

grep [options] 'search_condition' /file/
options to learn:
	-i      will make search condition case insensitive
	-r      will make it recursive and search condition in folder
	-v     will preform inverse match search
	-w    will perform a match to the word on its own and not include the string pattern in other           words 
	-o    will not display context (line) around the search and will display the match on its own
Basic regex:
	`^`      (example context `'^sam'`) will display search terms beginning with sam
	`$`      (example context `'sam$'`) will display search terms ending with sam
	`.`      (example context `'sa.'`) will display search terms with sa and exactly 1 *any* random character (cannot be void of characters or whitespace)
     `\`      (example context  `'\.'`) will search for the special character that proceeds backslash. this will allow you to search for period, asterisk etc. without accidentally searching for all character.
     `*`     (example context `'let*'`) will match the character that precedes the asterisk any number of times, including 0. this will match le, let, lett, lettt etc.
     **note**: a combination use case `'/.*/` would search for terms that match a directory structure / following any number of any characters and then closing with another /
Extended regex:
     `{}`   (example context `'0{1,3}'`) will search for the preceding character within the range in the syntax of {min,max}. The example will search for 0 within 0 and 000 and not match absent or exceeding 4. you can set just a minimum or just a maximum. A single value {*n*} will search for exactly *n* times.
     `?`    (example context `'disabled?'`) will make the preceding character *optional* which, in the context will search for disable or disabled
     `|`    (example context `'enable|disable'`) will search for either term on either side of the pipe operation, both will match
     `[]`  (example context `[a-z]`) will search for any of the values in the brackets or values in-between if - is used (such as `[0-9`)
     `()`  (example context `'/dev/([a-z]*[0-9]?)*'`) will work in a similar way to mathematical operation of the brackets, it will apply the operation within first and then it will apply the outer expression after to the whole inner expression. In this example we will look in /dev/(any character any times then any digit any times) any times
     `[^]` (example context `'http[^s]'`) will apply a negated match to the function following the caret within the brackets. In this example will will search for anything http not including https. This function can work with ranges or sets.


# (S)Tar
Creating tar archive with a file:
Tar -cf archive.tar file1
(c = create f = file)