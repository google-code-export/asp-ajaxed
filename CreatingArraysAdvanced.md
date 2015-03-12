# Introduction #

This article originally written by Michal Gabrukiewicz on 2008-08-08 at [WebDevBros](http://www.webdevbros.net/cool-way-of-creating-arrays-in-asp-vbscript-advanced/)


# Details #

It’s a small function which I will introduce today but very useful. When working recently on ajaxed I discovered that its often possible to pass an array or a primitive type as an argument to a function. Lets say you have a function `println(msg)` which prints out its argument as a line to the screen. When the argument is an array then it treats each field as a line. So it should be possible to call the function with those variations:
```
println("something")
println(array("some", "more", "lines"))
```

Lets see how we solve the solution in a nice way and how we will be able to create a function which is simply called `[]` (which is common syntax for an array in other languages).

First we create the `println` function which prints out the line(s):
```
sub println(line)
  lines = arrayize(line)
  for each l in lines
    response.write(l)
  next
end sub
```

As you can see we use an `arrayize` function internally which will ensure a variable to be an array. This little function does the job and helps us to keep our code nice and simple:
```
function arrayize(value)
  if isArray(value) then
    arrayize = value
  elseif isEmpty(value) then
    arrayize = array()
  else
    arrayize = array(value)
  end if
end function
```

Its straightforward but very effective. This function ensures a variable to be an array. If its already an array then its just passed trhough, otherwise the value is take as the first field of the array. In case of an `empty` variable it returns an empty array.

Thats it! But we go a step further and create an alias for it which will make the usage even more simpler. Lets check this:
```
function [](value)
  [] = arrayize(value)
end function
```

Exactly, we use the squared brackets (which are normally used to escape reserved words). Back to our first function it looks like this now:
```
sub println(line)
  for each l in [](line)
  response.write(l)
  next
end sub
```

I dont know about you guys, but i am very excited about this way. It makes my code much clearer now. Btw: This will be part of the version 2.0 of ajaxed.