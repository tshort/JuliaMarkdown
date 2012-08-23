# This is a test of Julia MD pages

## Basic code blocks

Here is a basic [Julia](http://www.julialang.org) input section. When the page is
calculated (hit the `calculate` button), the output of each Julia
section will appear below the input. Here's some example Markdown:

    ```julia
    a = randn(12)
    ```
Here's the result in the page:

```julia
a = randn(12)
```

Here's another entry:

```julia
b = 99.99 * a[1]
```

And another:

```julia
{:a => [1:30], :b => randn(9), "c" => ["a", "b"]}
```
Here's a plot (used D3 like the web REPL):

```julia
plot(cos, 0, 10)
```

Here's an example of Markdown output from Julia:


```julia  output=markdown
println("## This is a second-level heading")
println("This is a normal paragraph with *emphasis*.")
println()
println("* bullet 1")
println("  * bullet 1a")
println("* bullet 2")
```


[[[Calculate]]]

## Form elements

You can also enter form inputs using a Markdown
[extension](https://github.com/brikis98/wmd). Here are some examples
of form elements:
****
```
name = ___(Tom)
sex = (x) male () female
phones = [] Android [x] iPhone [x] Blackberry
city = {BOS, SFO, (NYC)}
```
****

Here are the form elements that result from the Markdown above.

name = ___(Tom)

sex = (x) male () female

phones = [] Android [x] iPhone [x] Blackberry

city = {BOS, SFO, (NYC)}

Each of the form labels above (`name`, `sex`, and `city`) can be used as Julia
variables. Here is a simple example:

```julia 
println("name = ", name)
println("sex = ", sex)
println("android = ", android)
println("city = ", city)
```

