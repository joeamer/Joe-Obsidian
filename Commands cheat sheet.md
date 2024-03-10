# **e/grep:**

grep [options] 'search_condition' /file/
options to learn:
	-i      will make search condition case insensitive
	-r      will make it recursive and search condition in folder
	-v     will preform inverse match search
	-w    will perform a match to the word on its own and not include the string pattern in other           words 
	-o    will not display context (line) around the search and will display the match on its own
(search condition) Basic regex:
	`^`      (example context `'^sam'`) will display search terms beginning with sam
	`$`      (example context `'sam$'`) will display search terms ending with sam
	`.`      (example context `'sa.'`) will display search terms with sa and exactly 1 *any* random character (cannot be void of characters or whitespace)
     `\`      (example context  `'\.'`) will search for the special character that proceeds backslash. this will allow you to search for period, asterisk etc. without accidentally searching for all character.
     `*`     (example context `'let*'`) will match the character that precedes the asterisk any number of times, including 0. this will match le, let, lett, lettt etc.
     **note**: a combination use case `'/.*/` would search for terms that match a directory structure / following any number of any characters and then closing with another /
(search condition) Extended regex:
*note: egrep is best used altogether as it will will allow all of the extended expressions without the need of the -e option*
     `{}`   (example context `'0{1,3}'`) will search for the preceding character within the range in the syntax of {min,max}. The example will search for 0 within 0 and 000 and not match absent or exceeding 4. you can set just a minimum or just a maximum. A single value {*n*} will search for exactly *n* times.
     `?`    (example context `'disabled?'`) will make the preceding character *optional* which, in the context will search for disable or disabled
     `|`    (example context `'enable|disable'`) will search for either term on either side of the pipe operation, both will match
     `[]`  (example context `[a-z]`) will search for any of the values in the brackets or values in-between if - is used (such as `[0-9`)
     `()`  (example context `'/dev/([a-z]*[0-9]?)*'`) will work in a similar way to mathematical operation of the brackets, it will apply the operation within first and then it will apply the outer expression after to the whole inner expression. In this example we will look in /dev/(any character any times then any digit any times) any times
     `[^]` (example context `'http[^s]'`) will apply a negated match to the function following the caret within the brackets. In this example will will search for anything http not including https. This function can work with ranges or sets.


# (S)Tar
List files within a tar file
`tar -tf archive.tar`                          - this will output files within the archive.tar
(t = list f = file)

Creating tar archive with a file:
`tar -cf archive.tar file1`                - this will output an archive.tar file with the file(s) specified.                                                                   You can use a (relative and absolute) directory.
(c = create f = file)
*you can also use zipping utilities with tar*
`tar -czf archive.tar.gz file1`
`tar -cjf archive.tar.bz2 file1`
`tar -cJf archive.tar.xz file1`
`tar -caf archive.tar.gz file1`        -this is the auto compress flag and will compress with the                                                                    matching extension.
Adding files to archive:
`tar -af archive.tar file2`               - this will add file2 to the archive.tar file

Extracting files:
`tar -xf archive.tar` (now its important to note that id tar was packed with a relative directory such as `tar -cf archive.tar Documets/` will result in extracting into the 'current directory'/Documents/file1)
`tar -xf archive.tar --directory /tmp/` will result in extracting to the directory specified rather than the one you are in
***note: if you want to ensure file permissions are preserved, you will need to run this as root***
***Another note: xf will work on zipped archives as well. It will automatically use the correct utility to decompress and extract***

Using star:
Star requires you to use the - before the options e.g. `star -cf` (whereas tar doesn't actually require you to), also, to specify the file you need to use `file=path/to/archive.star file`

Creating a star archive with a file:
`star -cv file=/home/bob/archive.star file1`
(where c is create and v is verbose)
*if you want to use compression*
`star -cv -z file=/home/bob/archive.star.gz file1`
`star -cv -bz file=/home/bob/archive.star.bz2 file1`
listing a star archive:
`star -tv file=/home/bob/archive.star`

extracting a star archive:
`star -xv file=/home/bob/archive.star`
*or to set directory output*
`star -xv file=/home/bob/archive.star -C /tmp/`
***decompression is automatic again so no need to apply any extra flags to unpack and decompress archive.star.gz/ bz2***

# Compressing
to zip contents of a file you can use Bzip2, Gzip or Xzip (preloaded zip utilities)

for zipping you can use any of:
`gzip file1`          -this will result in file1 turning into file1.gz
`bzip2 file1`        -this will result in file1 turning into file1.bz2
`xz file1`             -this will result in file1 turning into file1.xz

you can use the -k / --keep flag to keep the file after zipping `gzip -k file1`
-l / --list will show the compression ratio and file sizes `gzip -l file1`
-r will be used to compress directories `gzip -r Pictures/`

for unzipping you can use any of:
`gunzip file1.gz`                   | this is also eq to `gzip --decompress file1.gz`
`bunzip file1.bz2`                 | this is also eq to `bzip2 --decompress file1.bz2`
`xunzip file1.xz`                   | this is also eq to `xz --decompress file1.xz`

# Redirection

The `>` will redirect all output into the right e.g. `echo 'hello world > hello_wrld.txt` echo would normally output to the screen but instead the contents (hello world) is redirected to hello_wrld.txt. This will overwrite if the file exists already and all its contents

The `>>` will append redirect into the right e.g. `echo 'hello' >> hello_wrld.txt` this will then add hello on a new line under hello world for the file (if you have done the previous first). This will create the file if it doesn't exist

The `1>` will redirect standard output to the right, so this will do the same result as `>` example but in a situation where you are searching with grep in a folder structure where you don't have permissions to some of the files, and want to save the results to a file (for example) the error lines indicating insufficient permissions will be filtered out and display on the screen but the rest will be stored.

The `2>` will redirect standard error to the right. You can use this if you are again searching with grep and want to display on the screen this time. If you want to hide the error lines you can do `grep -r 'bob' /home 2> /dev/null/` . This will display all matching terms and the potential error will be redirected to a special folder location which will essentially put it to a null output... Straight to the shadow realm.

you can use multiple redirections at the same time under the similar syntax of: `grep -r 'bob' /home 1>output.txt 2>errors.txt` 

If you want to redirect all output you will need to do: ``grep -r 'bob' /home >allout.txt 2>&1`
this follows the logic of std out will go to the txt and then the std error will go to std out which goes to the txt again.

The `<` can be used to redirect files to functions such as `sendmail someone@example.com < emailcontent.txt` this example 'tricks' the sendmail function that the text input, you would normally type, is the text file with the pre-made content

You can use `<<` to add The here document is used when one wants to give multiple lines of input to the command or script.

And to use it, you'd have to follow the simple syntax:

command <<DELIMITER
Here, the DELIMITER is used two times.

Once in the command which you saw above.

And in the end when you're done adding lines and want to save the changes. The DELIMITER can be any string of characters but I would recommend using END as it represents the meaning much better!

Now, let's have a look at a simple example:

cat > output.txt <<END
Here, I have used the redirection between the cat command and the output.txt text file which means the output of the cat command will be sent to the file.

Then, I used << with the END delimiter which will let me write multiple lines and as soon as I type END, it will stop the command.