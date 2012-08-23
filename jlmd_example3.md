# More on Form Elements

## Basic form elements

See the Markdown extension [here](https://github.com/brikis98/wmd) for
documentation on how to enter forms. The basic four types of forms
supported are:

* text boxes -- assigns a string to a Julia variable. Two optional
  parameters can be specified as `___(default)[width]`. `default` is
  the default value; `width` is the width of the text box (defaults to
  20).
* radio buttons -- assigns a string to a Julia variable based on which
radio button is selected. An `x` marks the item selected initially.
* check boxes -- assigns `true` or `false` to each item checked in
  Julia. The name of the collection is not used. An `x` marks
  initially selected elements.
* dropdown select input -- assigns a string to a Julia variable based
on which item is selected. Parens mark the default item.

Here are examples.

```
name = ___(Tom)
sex = (x) male () female
phones = [] Android [x] iPhone [x] Blackberry
city = {BOS, SFO, (NYC)}
```

The example above will generate the following String variables in
Julia: `name`, `sex`, and `city`. The checkboxes will generate the
following Boolean variables: `android`, `iphone`, and `blackberry`
(these names are all converted to lowercase). Here is a simple example
using these variables:

name = ___(Tom)

sex = (x) male () female

phones = [] Android [x] iPhone [x] Blackberry

city = {BOS, SFO, (NYC)}

[[[Calculate]]]

```julia 
println("name = ", name)
println("sex = ", sex)
println("blackberry = ", blackberry)
println("city = ", city)
```

## Other form elements

For form elements beyond the basics, you can enter HTML. Here is an
example of a textarea to be used to hold Julia commands that the user
can enter:

```
<textarea name = "mytext" rows="4" cols="50">
println("hello world")
plot(sin, 0, 10.)
</textarea>
```

The value of the `name` argument becomes the Julia variable name
that will hold the contents of the textarea.

<textarea name = "mytext" rows="4" cols="50">
println("hello world")
plot(sin, 0, 10.)
</textarea>

Now, we can `eval()` that to allow the user to enter commands:

```julia
function evaltext(txt)
    txts = split(txt, "\n")
    for s in txts
        if length(s) > 0
            try
                eval(parse(s)[1])
            end
        end
    end
end
evaltext(mytext)
```

For more advanced input or output widgets, you can use elements from
Javascript toolkits. The web REPL and the JLMD interface uses the
[jQuery](jquery.com) toolkit. The [jQuery UI](jqueryui.com) code is
loaded by default in `jlmd.htm`. The examples below show a date picker
and a slider. For any javascript widget to work with Julia, the
resulting value needs to end up in a standard HTML form element. The
date picker already uses a text input box. The slider does not by
default, so when creating the slider, you have to update a form
element (in this case, the `#sliderinput` text input).

<script>
$(document).ready(function() {
    $("#datepicker").datepicker();
    $("#slider").slider({value:100, min:0, max:500, step: 5,
        change: function(event,ui) {console.log(ui.value);$("#sliderinput").val(ui.value);}});
  });
</script>

Date picker: <input id="datepicker" name="datepicker" type="text" value="08/01/2012">
  
Slider value: <input id="sliderinput" name="sliderinput" type="text" value="100">
<div id="slider"></div>

Now, we can use these in Julia:

```julia
println(datepicker)
println(sliderinput)
```
