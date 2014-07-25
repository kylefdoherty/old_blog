---
layout: post
title: "Ruby Notes - Working with Files"
date: 2014-02-17 22:56:05 +0100
comments: true
categories: Ruby
---

My next “Ruby Notes” post was going to be on arrays but in my last couple of mini projects, <a href="https://github.com/kylefdoherty/ruby-quiz-solutions/tree/master/text_munger_76" target="_blank">Text Munger problem on RubyQuiz</a>, and building an <a href="https://github.com/kylefdoherty/image-editor" target="_blank">image editor command line app</a>, I had to work a lot with files and directories and realized I didn’t have a great handle on them.  To remedy this I did what I always do, a bunch of reading, practice problems, and put it all down in my notebook.  There still is a lot for me to learn but I think this lays a good foundation for understanding and working with files in Ruby.
<!-- more -->
<h2>IO Class</h2>
The IO class is the parent class for the File class and thus is where it gets a ton of its methods such as readlines and readline.  IO stands for input/output, specifically input/output streams which are sequences of data that allow you to do things like play sound on your speakers and print output to a screen.  The IO class allows you to initialize streams and do things with them. 

<h2>Standard Output, Input, and Error</h2>
STDOUT, STDIN, and STDERR are ruby constants that are IO objects pointing to your programs output, input, and error streams.  You can access these streams through the terminal without opening any files.

When you do something like call puts, output is sent to the IO object that STDOUT points to.  Conversely when you call get, input is captured by the IO object that STDIN points to.

<strong>Further Reading:</strong> <a href="https://rubymonk.com/learning/books/1-ruby-primer/chapters/42-introduction-to-i-o/lessons/89-streams" target="_blank">https://rubymonk.com/learning/books/1-ruby-primer/chapters/42-introduction-to-i-o/lessons/89-streams</a>

<h2>File Class</h2>
According to the <a href="http://ruby-doc.org/core-1.9.3/File.html" target="_blank">ruby doc</a>, a File is an abstraction of any file object accessible by the program and is closely associated with the class IO (it’s a subclass of IO).  

You use the File class to create files, read them, and write to them.  There are various modes that can be given to the File class telling it what its behaviour is i.e. can read it, can write to it, can do both, etc.  These modes are inherited from the IO class and are listed below.


</style>
<h3>Modes</h3>
<table style="width:700px">
	<tr>
	  <th>Mode</th>
	  <th>Meaning</th>		
	</tr>
	<tr>
	  <td>"r"</td>
	  <td>Read-only, starts at beginning of file  (default mode).</td>		
	 </tr>
	<tr>
	  <td>"r+"</td>
	  <td>Read-write, starts at beginning of file.</td>		
	</tr>
	<tr>
	  <td>"w"</td>
	  <td>Write-only, truncates existing file to zero length or creates a new file for writing.</td>		
	</tr>
	<tr>
	  <td>"w+"</td>
	  <td>Read-write, truncates existing file to zero length or creates a new file for reading and writing.</td>		
	</tr>
	<tr>
	  <td>"a"</td>
	  <td>Write-only, starts at end of file if file exists, otherwise creates a new file for writing.</td>		
	</tr>
	<tr>
	  <td>"a+"</td>
	  <td>Read-write, starts at end of file if file exists, otherwise creates a new file for reading and writing.</td>		
	</tr>
	<tr>
	  <td>"b"</td>
	  <td>Binary file mode (may appear with any of the key letters listed above). Suppresses EOL <-> CRLF conversion on Windows. And sets external encoding to ASCII-8BIT unless explicitly specified.</td>		
	</tr>
	<tr>
	  <td>"t"</td>
	  <td>Text file mode (may appear with any of the key letters listed above except "b").</td>		
	</tr>
</table>
<br>

<h2>Writing to a File</h2>
```ruby Writing to a File 
file = File.open(“text.txt”, “w”)
file.puts “Hello from #{$0}”
file.close

#=> writes “Hello from io.rb” to the file text.txt
```
On the first line I’m calling the <code>.open</code> method on the File class and passing it the file text.txt and the mode I want the file to use, “w”.  Next I’m using the <code>.puts</code> method to write to the file and passing it the text I want it to write to the file.  Note, that If we didn’t have a file text.txt in our directory, this script would have created it.

<h3>Using Block Notation</h3>
```ruby Writing to a File with Block Notation
File.open(“text.txt”,”w”){|file| file.puts”Hola from $0”}
```

Note that when passing a block to File you don’t have to close it because when the block is exited it closes the File for you.

<h2>Reading a File</h2>
```ruby Reading from a File
file = File.open(“lib/I_have_a_dream.txt”)
contents = file.read
puts contents 
file.close
```
This is pretty simple.  We’re opening the file we want to read with the <code>.open</code> method and storing it in the file variable.  Then we call the <code>.read</code> method on file and store it in contents and then puts the contents. 
<code>.read</code> starts reading from the place the last <code>.read</code> operation stopped. Here we’ve read the entire file and thus if below <code>puts contents</code> we tried to read the file again there would be nothing to read because we're at the end of the file.

<h3>Reading a File Block Notation</h3>
```ruby Reading from a File Block Notation
contents = File.open(“lib/I_have_a_dream.txt”, “r”){|file| file.read}
puts contents 
```

<h2>Closing Files</h2>
If you open a file make sure you close it, unless you’re passing File a block and then the block will close the file when it ends.

The reason you need to close files is it forces a “flush”, which means it pushes the data-to-be-written to where you want it to be.  This frees up memory for the rest of your program and ensures the file is available for other processes to access. 

<strong>Further Reading:</strong> <a href="http://ruby.bastardsbook.com/chapters/io/" target="_blank">http://ruby.bastardsbook.com/chapters/io/</a> 

<h2>More File Methods</h2>
We’ve already seen some file methods like .open and .close but here are some more useful ones. Checkout the ruby doc for File and IO for the rest of them.

<h3>.readlines & .readline</h3>
These two methods can be very handy when you want to read one line at a time.  This would be useful for instance if you are reading a comma delimited file. 

<h3>.readlines</h3> - takes in all the content of the file and stores each line as an element of an array.  From here you can iterate over each line using each.

```ruby Using Readlines 
File.open(read_file).readlines.each do |line|
puts line
end 
```
<h3>.readline</h3> - is a bit different in it only reads one line at a time and thus you need to keep advancing it forward in the file, which can be done with a <code>while</code> or <code>until</code> method.  

```ruby Using Readline 
file = File.open("lib/blood_sweat_tears.txt")
until file.eof?
   line = file.readline
   puts line
end
file.close
```

The reason you would want to <code>.readline</code> vs <code>.readlines</code> is because <code>.readlines</code> loads the entire contents of the file into memory.  For a small script working with small files this isn’t a problem but if you are using large files and/or have multiple users this is bad.

<h3>.exists?</h3> - checks for the existence of the file. 

```ruby 
if File.exists?(“file_name”)
	#do something 
end 
```

<h3>.absolute_path</h3> - gets the absolute path for the. 

``` ruby 
puts File.absolute_path("lib/blood_sweat_tears.txt")
#=> “/Users/kyledoherty/Dropbox/Ruby/learn_to_program/working_w_files/lib/blood_sweat_tears.txt”
```

<h3>.basename</h3> - gives you just the filename. 

``` ruby 
puts File.basename(“/Users/kyledoherty/Dropbox/Ruby/learn_to_program/working_w_files/lib/blood_sweat_tears.txt”)
#=> “blood_sweat_tears.txt’
```

<h3>.directory?</h3> - returns true if the string passed to it is a directory.

``` ruby 
Dir.open(Dir.pwd).each do |filename|
	next if File.directory? filename 
end 
```

<h2>Dir Class</h2>
The Directory class allows you to work with driectories as you’d expect.  Most of the methods you can use on the directory class are the same as the commands you use in the console. 

<h2>Some Dir Methods</h2>

<h3>.pwd</h3> - tells you what directory you’re in.

```ruby 
puts Dir.pwd
#=> "/Users/kyledoherty/Dropbox/Ruby/image_edit"  
```

<h3>.chdir</h3> -  allows you to change to a new directory.

```ruby 
Dir.chdir(“"/Users/kyledoherty/Dropbox/Ruby/rubyquiz”
#=> 0
```

<h3>.mkdir</h3> - makes a new directory named the string it is passed.


```ruby 
Dir.mkdir(“stuff”)
#=> 0
```

<h3>.rmdir</h3> - removes an empty directory but throws an error if it contains files.  To remove a directory with files you must use the FileUtils module.

```ruby 
Dir.rmdir(“stuff”)
#=> 0
```

<h2>Accessing Directory Content</h2>
There are two ways to grab content from directories, using .entries and .glob.  

<h3>.entries</h3> -  returns an array with every single entry inside the diretory including “.” and hidden files.

```ruby 
Dir.entries(“../rubyquiz”)
#=> [".", "..", ".DS_Store", ".git", "README", "text_munger_76"] 
```
<h3>.glob</h3> - can be passed a directory name or pattern such as <code>*.txt</code> and returns an array of just the visible files

```ruby 
Dir.entries(“*”)
#=> ["README", "text_munger_76"] 
```
Gives us the files in the current directory.

```ruby 
Dir.entries(“**/*.txt”)
#=> ["text_munger_76/lib/blood_sweat_tears.txt", "text_munger_76/lib/gettysburg_address.txt", "text_munger_76/lib/I_have_a_dream.txt", "text_munger_76/lib/pearl_harbor_address.txt", "text_munger_76/lib/strength_and_decency.txt"] 
```
Here we use <code>**/*.txt</code> to search the current directory and all it’s sub directories for any .txt files using a recursive search and passing it the pattern .txt.

<h2>FileUtils Module</h2>
I’m not going to go into FileUtils too much but it allows more control over files and mimics a lot of the command line commands and flags you can use such as <code>rm -rf</code> for removing directories that contain files.

<h2>Some Methods</h2>

<h3>.mkdir</h3> - makes a directory 

<h3>.touch</h3> - makes a file 

<h3>.rm_rf</h3> - removes a directory whether it contains other files and directories or not 

```ruby
if File.exists?(“file_name”)
FileUtils.rm_rf(“file_name”)
end  

``` 

Note: you need to require FileUtils in your files with <code>require ‘fileutils’</code>










